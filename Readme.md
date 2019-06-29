# Build AWS SAM Development Environment

Ref.
1. https://docs.aws.amazon.com/en_us/serverless-application-model/latest/developerguide/serverless-quick-start.html

## Installing the AWS SAM CLI on macOS

https://docs.aws.amazon.com/en_us/serverless-application-model/latest/developerguide/serverless-sam-cli-install-mac.html

~~~
$ brew tap aws/tap
==> Tapping aws/tap
Cloning into '/usr/local/Homebrew/Library/Taps/aws/homebrew-tap'...
remote: Enumerating objects: 6, done.
remote: Counting objects: 100% (6/6), done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 6 (delta 0), reused 5 (delta 0), pack-reused 0
Unpacking objects: 100% (6/6), done.
Tapped 1 formula (32 files, 42.1KB).
~~~
~~~
$ brew install aws-sam-cli
==> Installing aws-sam-cli from aws/tap
==> Downloading https://github.com/awslabs/aws-sam-cli/releases/download/v0.17.0
==> Downloading from https://github-production-release-asset-2e65be.s3.amazonaws
######################################################################## 100.0%
==> Pouring aws-sam-cli-0.17.0.sierra.bottle.tar.gz
🍺  /usr/local/Cellar/aws-sam-cli/0.17.0: 3,770 files, 56.2MB
~~~
~~~
$ sam --version
SAM CLI, version 0.17.0
~~~

# Debug AWS SAM in local using Golang sample code

Ref.
1. https://docs.aws.amazon.com/en_us/serverless-application-model/latest/developerguide/serverless-sam-cli-using-debugging-golang.html
2. https://github.com/awslabs/aws-sam-cli/issues/1067


## Make a AWS SAM Golang sample code

~~~
sam init --runtime go
cd PATH_TO/sam-app
go get -u github.com/go-delve/delve/cmd/dlv
GOARCH=amd64 GOOS=linux go build -o ./dlv github.com/go-delve/delve/cmd/dlv
go get -u github.com/aws/aws-lambda-go/...
GOARCH=amd64 GOOS=linux go build -gcflags='-N -l' -o hello-world/hello-world ./hello-world
sam local start-api -d 5986 --debugger-path . --debug-args="-delveAPI=2"
~~~

## VSCode launch.json

1. Open a workspace reference to PATH_TO/sam-app/hello-world
2. Switch to debug window, edit the config

~~~
{
    "version": "0.2.0",
    "configurations": [
    {
        "name": "Connect to Lambda container",
        "type": "go",
        "request": "launch",
        "mode": "remote",
        "remotePath": "",
        "port": "5986",
        "host": "127.0.0.1",
        "program": "${workspaceRoot}",
        "env": {},
        "args": [],
      },
    ]
}
~~~

## Hit the endpoint

~~~
http://localhost:3000/hello
~~~

the output in console.

~~~
sam local start-api -d 5986 --debugger-path . --debug-args="-delveAPI=2"
2019-06-29 22:17:58 Mounting HelloWorldFunction at http://127.0.0.1:3000/hello [GET]
2019-06-29 22:17:58 You can now browse to the above endpoints to invoke your functions. You do not need to restart/reload SAM CLI while working on your functions, changes will be reflected instantly/automatically. You only need to restart SAM CLI if you update your AWS SAM template
2019-06-29 22:17:58  * Running on http://127.0.0.1:3000/ (Press CTRL+C to quit)
2019-06-29 22:18:23 127.0.0.1 - - [29/Jun/2019 22:18:23] "GET / HTTP/1.1" 403 -
2019-06-29 22:18:24 127.0.0.1 - - [29/Jun/2019 22:18:24] "GET /favicon.ico HTTP/1.1" 403 -
2019-06-29 22:18:24 127.0.0.1 - - [29/Jun/2019 22:18:24] "GET /robots.txt?1561814303387 HTTP/1.1" 403 -
2019-06-29 22:18:41 Invoking hello-world (go1.x)
2019-06-29 22:18:41 Found credentials in shared credentials file: ~/.aws/credentials

Fetching lambci/lambda:go1.x Docker container image.....................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................
2019-06-29 22:21:37 Mounting /Users/WilliamTai/Documents/Git/AwsSamInitGo/sam-app/hello-world as /var/task:ro,delegated inside runtime container
Could not create config directory: mkdir .config: read-only file system.API server listening at: [::]:5986
2019-06-29T13:21:39Z info layer=debugger launching process with args: [/var/task/hello-world]
2019-06-29T13:25:23Z info layer=debugger created breakpoint: &api.Breakpoint{ID:1, Name:"", Addr:0x6e599d, File:"/Users/WilliamTai/Documents/Git/AwsSamInitGo/sam-app/hello-world/main.go", Line:26, FunctionName:"main.handler", Cond:"", Tracepoint:false, TraceReturn:false, Goroutine:false, Stacktrace:0, Variables:[]string(nil), LoadArgs:(*api.LoadConfig)(nil), LoadLocals:(*api.LoadConfig)(nil), HitCount:map[string]uint64{}, TotalHitCount:0x0}
2019-06-29T13:25:23Z info layer=debugger created breakpoint: &api.Breakpoint{ID:2, Name:"", Addr:0x6e59fe, File:"/Users/WilliamTai/Documents/Git/AwsSamInitGo/sam-app/hello-world/main.go", Line:30, FunctionName:"main.handler", Cond:"", Tracepoint:false, TraceReturn:false, Goroutine:false, Stacktrace:0, Variables:[]string(nil), LoadArgs:(*api.LoadConfig)(nil), LoadLocals:(*api.LoadConfig)(nil), HitCount:map[string]uint64{}, TotalHitCount:0x0}
2019-06-29T13:25:23Z debug layer=debugger continuing
START RequestId: 2e694067-bb0c-14ee-1b0e-8736f0ce0d7b Version: $LATEST
2019-06-29T13:25:51Z debug layer=debugger nexting
2019-06-29T13:25:54Z debug layer=debugger nexting
2019-06-29T13:25:57Z debug layer=debugger nexting
2019-06-29T13:25:58Z debug layer=debugger nexting
2019-06-29T13:26:00Z debug layer=debugger nexting
2019-06-29T13:26:05Z debug layer=debugger nexting
2019-06-29T13:26:06Z debug layer=debugger nexting
2019-06-29T13:26:07Z debug layer=debugger nexting
2019-06-29T13:26:08Z debug layer=debugger nexting
2019-06-29T13:26:11Z debug layer=debugger continuing
END RequestId: 2e694067-bb0c-14ee-1b0e-8736f0ce0d7b
REPORT RequestId: 2e694067-bb0c-14ee-1b0e-8736f0ce0d7b	Duration: 48263.27 ms	Billed Duration: 48300 ms	Memory Size: 128 MB	Max Memory Used: 42 MB	
2019-06-29 22:26:12 No Content-Type given. Defaulting to 'application/json'.
2019-06-29 22:26:12 127.0.0.1 - - [29/Jun/2019 22:26:12] "GET /hello HTTP/1.1" 200 -
2019-06-29 22:26:12 127.0.0.1 - - [29/Jun/2019 22:26:12] "GET /robots.txt?1561814772760 HTTP/1.1" 403 -
2019-06-29 22:26:14 127.0.0.1 - - [29/Jun/2019 22:26:14] "GET /favicon.ico HTTP/1.1" 403 -
~~~

## Set the break point in VSCode then run debugger

Set the break point in the Lamdba handler then run the debugger, it will running in remote mode.