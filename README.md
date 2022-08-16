# bob-quick-start

To quickly get started, clone our sample `Hello World` project:

```shell
git clone git@github.com:benchkram/bob-quick-start.git
cd bob-quick-start 
```

Inside `bob-quick-start`, you'll notice a `bob.yaml` file. This file, also called **Bobfile** contains the instructions
for all the build tasks of your project:

```yaml
build:
  build:
    input: |-
      main.go
      go.mod
      go.sum
    cmd: go build -o ./build/server main.go
    target: /build/server
dependencies:
  - go_1_18
```

Running `bob build` will build the entire project from scratch:

```shell
bob build

Building nix dependencies...
Succeeded building nix dependencies
Running task build with 0 dependencies

build      	running task...
build      	...done

● ● ● ●
Ran 1 tasks in 330ms 
  build              	✔       	(330ms)
```

The first thing you might have noticed is that you didn't have to install Go for this application to work. bob takes
care of installing all your project dependencies by using [Nix](https://nixos.org/) under the hood. Nix has over **80 000 packages** to pick from so
you no longer need to rely on Docker containers to manage your environment.

The second thing to notice is that the binary for our server has been built inside `./build` directory. You can now run it:

```shell
./build/server 
```

If you go to [http://localhost:3333/](http://localhost:3333/) you should see `Hello World!`.

If you run `bob build` again, now it's much faster because bob will cache the build outputs. If the first build
took `330ms`
now the second build only took `1ms`.

```shell
$ bob build     
Building nix dependencies...
Succeeded building nix dependencies
Running task build with 0 dependencies

● ● ● ●
Ran 1 tasks in 1 ms
  build              	cached  
```

Let's change the `main.go` file from `"Hello, World!"` to `"Hello, Bob!"`:

```go
func Hello(w http.ResponseWriter, r *http.Request) {
  fmt.Fprint(w, "Hello, Bob!")
}
```

Now when we run `bob build` again, we see the project is built again because `main.go` has changed:

```shell
Building nix dependencies...
Succeeded building nix dependencies
Running task build with 0 dependencies

build      	running task...
build      	...done

● ● ● ●
Ran 1 tasks in 341 ms
  build              	✔       	(340ms)
```

bob figured out that the source code has changed and rebuilt out binary server. 

