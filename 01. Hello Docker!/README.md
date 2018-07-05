### Hello Docker

Docker란 리눅스의 응용프로그램들을 소프트웨어 Container안에 배치시키는 일을 자동화하는 오픈 소스 프로젝트이다.

Docker의 홈페이지에는 기능을 아래와 같이 명시하고 있다.

> Docker Container는 일종의 소프트웨어 실행에 필요한 모든 것들을 포함하는 완전한 파일 시스템 안에 감싼다. 여기에는 코드, 런타임, 시스템 도구, 시스템 라이브러리 등 서버에 설치되는 무엇이든 전부 아우른다. 이는 실행 중인 환경에 관계 없이 언제나 동일하게 실행될 것을 보증한다.

### Monolithic에서 Microservice로

지금까지 Infrastructure를 기반으로 OS/Runtime/Middleware는 잘 정돈 되어져 있었고, 그 위에 수많은 기능들을 제공하는 Application이 실행되고 있었다. 하지만 Application의 이런 구조는 Mobile 기기를 필두로 한 전반적인 IT 산업의 변화에 있어서 독이 되었고, 이는 곧 Application의 유연성을 크게 해치게 되었다.

이런 흐름에 맞춰 Application들의 전체적인 구조는 모두 바뀌게 된다. 대형 Application들은 다양한 기기와 환경들에 맞춰 빠르게 지원하기 위해 잘게 쪼개졌으며, 개발자들은 최상의 성능을 위해서 사용 가능한 Service들을 개별적으로 조립하여 제공하였다. 또한, 조립된 Service들에 맞춰 Infrastructure도 Public / Private / Virtualized된 다양한 환경을 구성하여 사용하게 된다.

### 왜 Docker인가

이러한 변화 속에 새로운 과제들이 나타나기 시작한다.

* 다양한 방식으로 조립된 서비스가 일관되게 상호 작용할 수 있도록 보장하는 방법은 무엇인가?
* 각 Service별 `Dependency Hell`을 피할 수 있는 방법은 무엇인가?
* 수많은 Application을 다양한 환경으로 신속하게 Migration및 확장할 수 있는가?
* 각 환경에 배포된 Application들의 호환성이 보장 되는가?
* n개의 Service가 n개의 Infrastructure에서 잘 구동될 수 있도록 하는 수 많은 설정들을 어떻게 피할 수 있는가?

과제들에 대한 해결책으로 나타난 것이 **Container**이다. Container는 Library, 시스템 도구, Code, Runtime등 application에 필요한 모든 것이 포함되어 있는 표준화된 유닛이고 환경에 구애받지 않고 Application을 신속하게 배포 및 확장 할 수 있는 환경을 제공해 준다.

#### 따라서, Docker는 이런 Container들을 관리할 수 있는 Service를 제공함으로써, Application들이 다양한 환경 속에서 일관된 서비스를 제공할 수 있도록 보장한다.

### Docker를 사용한다면

1. 동일한 Application을 다양한 환경으로 빠르게 배포할 수 있다.

	Docker는 Application 및 Service를 Container를 사용하여 표준화된 환경에서 구동할 수 있도록 함으로써, 개발 주기를 단축시킨다.

2. 환경에 따라 배포와 확장이 자유롭다.

	Docker는 개발자의 Laptop, Datacenter의 VM, Cloud환경 또는 여러 다양한 환경에 쉽게 이식하여 사용할 수 있다. Docker의 이런 특성으로 인해, 비즈니스 요구 사항에 맞춰 Application과 Service를 부하에 따라 동적으로 관리 할 수 있고 거의 실시간으로 축소 또는 확장 가능하다. 타 Platform에 비해 같은 Hardware에서 더 많은 작업을 수행할 수 있다. Docker는 가볍고 빠르게 동작하기 때문에, Hypervisor 기반의 VM 보다 실용적이고 비용 효율적이다. 따라서, 적은 Resources로 많은 작업을 수행해야하는 중소규모의 배포 환경 및 고밀도 밀집 환경에 이상적이다.

### Hello Docker

1. Download

	* windows

		[Docker for windows](https://docs.docker.com/docker-for-windows/install/)

	* Mac

		[Docker for mac](https://docs.docker.com/docker-for-mac/install/)

	* Linux

		> $ curl -fsSL https://get.docker.com/ | sudo sh

2. `$ docker version`으로 설치된 Docker의 Version을 확인한다.

	~~~
	{18-07-05 14:05}jaejin-ui-MacBook-Pro:~/dev jaejin% docker version
	Client:
	 Version:      18.03.1-ce
	 API version:  1.37
	 Go version:   go1.9.5
	 Git commit:   9ee9f40
	 Built:        Thu Apr 26 07:13:02 2018
	 OS/Arch:      darwin/amd64
	 Experimental: false
	 Orchestrator: swarm

	Server:
	 Engine:
	  Version:      18.03.1-ce
	  API version:  1.37 (minimum version 1.12)
	  Go version:   go1.9.5
	  Git commit:   9ee9f40
	  Built:        Thu Apr 26 07:22:38 2018
	  OS/Arch:      linux/amd64
	  Experimental: true
	{18-07-05 14:08}jaejin-ui-MacBook-Pro:~/dev jaejin% docker-compose version
	docker-compose version 1.21.1, build 5a3f1a3
	docker-py version: 3.3.0
	CPython version: 3.6.4
	OpenSSL version: OpenSSL 1.0.2o  27 Mar 2018
	{18-07-05 14:08}jaejin-ui-MacBook-Pro:~/dev jaejin% docker-machine version
	docker-machine version 0.14.0, build 89b8332
	~~~

3. 구성된 Docker 환경에 Server와 Client외에도 docker-compose와 docker-machine Tool도 같이 설치된 것을 확인 할 수 있다.
4. `docker run` 명령어로 `hello-world` Image를 pull받아서 Container생성하여 실행한다.

	```
	{18-07-05 14:08}jaejin-ui-MacBook-Pro:~/dev jaejin% docker run hello-world
	Unable to find image 'hello-world:latest' locally
	latest: Pulling from library/hello-world
	9bb5a5d4561a: Pull complete
	Digest: sha256:3e1764d0f546ceac4565547df2ac4907fe46f007ea229fd7ef2718514bcec35d
	Status: Downloaded newer image for hello-world:latest

	Hello from Docker!
	This message shows that your installation appears to be working correctly.

	To generate this message, Docker took the following steps:
	 1. The Docker client contacted the Docker daemon.
	 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
	    (amd64)
	 3. The Docker daemon created a new container from that image which runs the
	    executable that produces the output you are currently reading.
	 4. The Docker daemon streamed that output to the Docker client, which sent it
	    to your terminal.

	To try something more ambitious, you can run an Ubuntu container with:
	 $ docker run -it ubuntu bash

	Share images, automate workflows, and more with a free Docker ID:
	 https://hub.docker.com/

	For more examples and ideas, visit:
	 https://docs.docker.com/engine/userguide/
	```

5. 위와 같이 정상적으로 메시지가 나타나면 Docker환경 구성이 완료된 것이다.

### 한 번의 Build로 어디서든 Run

지금까지 Docker환경을 구성하여 `hello-world`라는 Image로부터 Container를 구동시켰다. Docker가 설치되어 있다면, `hello-world`는 어디에서든지 100% 동일한 형태로 실행할 것이다. 이는 Application들이 더 이상 환경에 제약받지 않고, 일관된 서비스를 제공할 수 있음을 의미한다.
