---
title: "(CI/CD, Github Actions) Github Actions를 활용한 SSH 접속, git push 코드 자동 배포"

categories:
   - study

tags:
   - study
   - Backend
   - github actions
   - git
   - SSH
   - DevOps

comments: true
toc: true
toc_sticky: true

date: 2022-07-28 17:22 +0900
last_modified_at: 2022-07-28 17:22 +0900
---

***CI/CD***란 각각 CI(Continuous Integration/지속적 통합), CD(Continuous Deployment/지속적 배포)의 약어로, 코드를 수정하고 테스트 후 빌드, 배포 하는 과정을 개발자의 수동적인 개입 없이, 자동으로 진행하는 것을 의미한다.<br><br>
이번 글에는 진짜 단순히 나의 편의를 위해 작성한, Github Actions를 활용한 CI 과정을 소개해 보려고 한다.<br><br>
현재 내가 사용중인 코드는 SSH 터널링, AWS EC2 Security Group 수정, SLACK 배포 완료 / 배포 성공 알림 과정이 포함되어 있지만, 이 부분을 추가하면 내용이 길어질 것 같으므로 아래의 간단한 과정으로 진행해보려고 한다.
1. Github Actions 설정
2. Actions secrets 설정
3. Workflow 파일 작성 (ssh, git pull, ci skip 분기)
4. 실행 결과 (성공, 실패, 스킵)

<br>
이렇게 진행하는 게 정답이 아닐 수도 있지만, 로컬에서 개발 서버로 배포하는 시간만 체감상 3분 넘게 줄일 수 있다. 점점 코드를 수정해 나가야지..
<br><br>

# 1. __Github Actions__
[*Github Actions*](https://github.com/features/actions)를 활용하는 과정에 알아야 하는 용어들과 개념은 대표적으로 Workflow, Event, Job, Step, Runner가 있다. 수많은 블로그에서 이 내용을 다뤘을 것이므로, 간단하게 한 줄씩만 쓰고 넘어갈 것이다.<br>
* Workflow : 가장 최상위 개념으로, .github/workflows 폴더 안에 .yaml 파일로 저장된다.
* Event : push나 pr 등 Workflow를 실행하기 위한 활동들이다.
* Jobs : 하위 개념인 steps 들이 묶어져 하나의 job이 된다.
* Steps : 실제로 실행되는 내용이 정의되어 있는 블록이고, name, run, with 등으로 정의한다.
* Runner : 위의 Workflow를 실행하는 주체이다. 직접 host중인 인스턴스를 정의할 수 있고, 기본적으로는 github에서 제공하는 인스턴스가 활용된다.

<br>

Github Actions의 Workflow를 작성하는 방법은 두 가지가 있다.<br>

1. github actions에서 제공하는 템플릿을 수정
2. 직접 .github/workflows 폴더 안에 파일 작성

<br>
어차피 나는 2번을 할 것이지만, 그래도 1번을 한번 살펴보면 다음과 같다.<br><br>
본인의 repo에서 Actions 탭에 들어가면, (한 번도 workflow를 생성하지 않았다면) 아래와 같이 Get Started 창이 나타난다.<br>

![image](https://drive.google.com/uc?id=1EtDXj_qHgU-h1zZOu-c4Ids1UoQWyRz2)

<br>
원하는 워크플로우에 Configure 버튼을 누르면 다음과 같이 .yaml 코드 프리셋이 작성되어 있다.<br><br>

<div class="colorscripter-code" style="color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position:relative !important;overflow:auto"><table class="colorscripter-code-table" style="margin:0;padding:0;border:none;background-color:#fafafa;border-radius:4px;" cellspacing="0" cellpadding="0"><tr><td style="padding:6px;border-right:2px solid #e5e5e5"><div style="margin:0;padding:0;word-break:normal;text-align:right;color:#666;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="line-height:130%">1</div><div style="line-height:130%">2</div><div style="line-height:130%">3</div><div style="line-height:130%">4</div><div style="line-height:130%">5</div><div style="line-height:130%">6</div><div style="line-height:130%">7</div><div style="line-height:130%">8</div><div style="line-height:130%">9</div><div style="line-height:130%">10</div><div style="line-height:130%">11</div><div style="line-height:130%">12</div><div style="line-height:130%">13</div><div style="line-height:130%">14</div><div style="line-height:130%">15</div><div style="line-height:130%">16</div><div style="line-height:130%">17</div><div style="line-height:130%">18</div><div style="line-height:130%">19</div><div style="line-height:130%">20</div><div style="line-height:130%">21</div><div style="line-height:130%">22</div><div style="line-height:130%">23</div><div style="line-height:130%">24</div><div style="line-height:130%">25</div><div style="line-height:130%">26</div><div style="line-height:130%">27</div><div style="line-height:130%">28</div><div style="line-height:130%">29</div><div style="line-height:130%">30</div><div style="line-height:130%">31</div><div style="line-height:130%">32</div></div></td><td style="padding:6px 0;text-align:left"><div style="margin:0;padding:0;color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="padding:0 6px; white-space:pre; line-height:130%"><span style="color:#999999">#&nbsp;This&nbsp;workflow&nbsp;will&nbsp;do&nbsp;a&nbsp;clean&nbsp;installation&nbsp;of&nbsp;node&nbsp;dependencies,&nbsp;cache/restore&nbsp;them,&nbsp;build&nbsp;the&nbsp;source&nbsp;code&nbsp;and&nbsp;run&nbsp;tests&nbsp;across&nbsp;different&nbsp;versions&nbsp;of&nbsp;node</span></div><div style="padding:0 6px; white-space:pre; line-height:130%"><span style="color:#999999">#&nbsp;For&nbsp;more&nbsp;information&nbsp;see:&nbsp;https://help.github.com/actions/language-and-framework-guides/using-nodejs-with-github-actions</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;</div><div style="padding:0 6px; white-space:pre; line-height:130%">name:&nbsp;Node.js&nbsp;CI</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;</div><div style="padding:0 6px; white-space:pre; line-height:130%">on:</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;push:</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;branches:&nbsp;[&nbsp;<span style="color:#63a35c">"master"</span>&nbsp;]</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;pull_request:</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;branches:&nbsp;[&nbsp;<span style="color:#63a35c">"master"</span>&nbsp;]</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;</div><div style="padding:0 6px; white-space:pre; line-height:130%">jobs:</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;build:</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;runs<span style="color:#0086b3"></span><span style="color:#a71d5d">-</span>on:&nbsp;ubuntu<span style="color:#0086b3"></span><span style="color:#a71d5d">-</span>latest</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;strategy:</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;matrix:</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;node<span style="color:#0086b3"></span><span style="color:#a71d5d">-</span>version:&nbsp;[<span style="color:#0099cc">12.</span>x,&nbsp;<span style="color:#0099cc">14.</span>x,&nbsp;<span style="color:#0099cc">16.</span>x]</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#999999">#&nbsp;See&nbsp;supported&nbsp;Node.js&nbsp;release&nbsp;schedule&nbsp;at&nbsp;https://nodejs.org/en/about/releases/</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;steps:</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">-</span>&nbsp;uses:&nbsp;actions<span style="color:#0086b3"></span><span style="color:#a71d5d">/</span>checkout@v3</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">-</span>&nbsp;name:&nbsp;Use&nbsp;Node.js&nbsp;${{&nbsp;matrix.node<span style="color:#0086b3"></span><span style="color:#a71d5d">-</span>version&nbsp;}}</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;uses:&nbsp;actions<span style="color:#0086b3"></span><span style="color:#a71d5d">/</span>setup<span style="color:#0086b3"></span><span style="color:#a71d5d">-</span>node@v3</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;with:</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;node<span style="color:#0086b3"></span><span style="color:#a71d5d">-</span>version:&nbsp;${{&nbsp;matrix.node<span style="color:#0086b3"></span><span style="color:#a71d5d">-</span>version&nbsp;}}</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;cache:&nbsp;<span style="color:#63a35c">'npm'</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">-</span>&nbsp;run:&nbsp;npm&nbsp;ci</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">-</span>&nbsp;run:&nbsp;npm&nbsp;run&nbsp;build&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">-</span><span style="color:#0086b3"></span><span style="color:#a71d5d">-</span><span style="color:#a71d5d">if</span><span style="color:#a71d5d">-</span>present</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">-</span>&nbsp;run:&nbsp;npm&nbsp;test</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;</div></div><div style="text-align:right;margin-top:-13px;margin-right:5px;font-size:9px;font-style:italic"><a href="http://colorscripter.com/info#e" target="_blank" style="color:#e5e5e5text-decoration:none">Colored by Color Scripter</a></div></td><td style="vertical-align:bottom;padding:0 2px 4px 0"><a href="http://colorscripter.com/info#e" target="_blank" style="text-decoration:none;color:white"><span style="font-size:9px;word-break:normal;background-color:#e5e5e5;color:white;border-radius:10px;padding:1px">cs</span></a></td></tr></table></div><br>

위의 코드를 토대로 Github Actions를 설명해 보면 다음과 같다.
1. 4번째 줄 : 해당 워크플로우의 이름을 지정해 준다. 나중에 Actions 탭에서 나타나게 될 작업 이름이다.
2. 6~10번째 줄 : 해당 워크플로우가 실행되는 event이다. 위의 코드의 경우 "master" branch에서 push 혹은 pr이 일어났을 경우 동작한다.
3. 12~ 번째 줄 : jobs가 정의되는 부분이다. build 아래에 인스턴스의 이미지를 정의해 줄 수 있다.
4. 22~ 번째 줄 : jobs가 실행되는 순서이다. 23 번째 줄과 같이 사용할 모듈을 정의해줄 수도 있고, run 커맨드에 의해 순서대로 실행된다.

저 상태로 Start commit 버튼을 클릭하면 그대로 파일이 생성되고, .github/workflows 폴더에 .yaml 파일이 생성되는 것을 확인할 수 있다.<br><br>

나는 직접 .yaml 파일을 만드는 게 더 편해서(형식은 같다), 프로젝트 폴더에 .github/works 폴더를 만든 후 .yaml 파일을 만든 후 commit/push 할 예정이다.<br><br>

workflow 파일을 작성하기 전, 배포되어서는 안되는 정보들을 Actions secrets에 넣는 과정을 먼저 진행해 보자.<br>

# 2. __Actions secrets__
github에서는 code에 배포되어서는 안되는 secret 한 내용들을 위해 secret params를 저장할 공간을 제공하는데, 각 repo의 Settings - Secrets 탭에서 확인할 수 있다.<br><br>
한번도 secret params를 만든 적이 없으면, 아래의 이미지처럼 나타난다.

![image2](https://drive.google.com/uc?id=1F2KpW-I7NCIFKtGkRpkUUiq6Pgp299RD)

<br>
New repository secret을 눌러 Host IP 혹은 port 등 secret params들을 입력해 주자.<br><br>

![image3](https://drive.google.com/uc?id=1F4v7xZECvztJPUerfDDcb_5rb1AJyrln)

<br>
참고로 이 글에서 활용하고자 하는 ssh 스크립트는 [appleboy](https://github.com/appleboy/ssh-action) 의 스크립트이다. 필수적으로 필요한 항목은 ssh key, host, username, port 이니, 해당 항목들은 필수적으로 추가해주도록 하자.<br><br>

추가한 이후엔 내용을 확인할 수는 없고 값을 수정/삭제만 가능하다. 본인이 활용할 이름과 값으로 잘 만들어 주자.
<br>

# 3. __Workflow 작성__
나의 경우에는 로컬에서 "dev" branch를 pull한 후 작업 후 push 하면 ec2 서버에서 git pull 명령어가 실행되게끔 하는게 목표이므로, 직접 아래와 같이 파일을 작성했다.<br><br>
다만 커밋 메시지에 [skipci] 혹은 [ciskip]라는 문구가 포함되면 CI 과정을 건너뛰게끔 하기 위해, if를 활용해 추가해 주었다.<br><br>

<div class="colorscripter-code" style="color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position:relative !important;overflow:auto"><table class="colorscripter-code-table" style="margin:0;padding:0;border:none;background-color:#fafafa;border-radius:4px;" cellspacing="0" cellpadding="0"><tr><td style="padding:6px;border-right:2px solid #e5e5e5"><div style="margin:0;padding:0;word-break:normal;text-align:right;color:#666;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="line-height:130%">1</div><div style="line-height:130%">2</div><div style="line-height:130%">3</div><div style="line-height:130%">4</div><div style="line-height:130%">5</div><div style="line-height:130%">6</div><div style="line-height:130%">7</div><div style="line-height:130%">8</div><div style="line-height:130%">9</div><div style="line-height:130%">10</div><div style="line-height:130%">11</div><div style="line-height:130%">12</div><div style="line-height:130%">13</div><div style="line-height:130%">14</div><div style="line-height:130%">15</div><div style="line-height:130%">16</div><div style="line-height:130%">17</div><div style="line-height:130%">18</div><div style="line-height:130%">19</div><div style="line-height:130%">20</div><div style="line-height:130%">21</div><div style="line-height:130%">22</div><div style="line-height:130%">23</div><div style="line-height:130%">24</div><div style="line-height:130%">25</div></div></td><td style="padding:6px 0;text-align:left"><div style="margin:0;padding:0;color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="padding:0 6px; white-space:pre; line-height:130%">name:&nbsp;deploy&nbsp;dev&nbsp;code</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;</div><div style="padding:0 6px; white-space:pre; line-height:130%">on:</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;push:</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;branches:&nbsp;[&nbsp;<span style="color:#63a35c">"dev"</span>&nbsp;]</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;</div><div style="padding:0 6px; white-space:pre; line-height:130%">jobs:</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;SSH:</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;runs<span style="color:#0086b3"></span><span style="color:#a71d5d">-</span>on:&nbsp;ubuntu<span style="color:#0086b3"></span><span style="color:#a71d5d">-</span>latest</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#a71d5d">if</span>:&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">&gt;</span><span style="color:#0086b3"></span><span style="color:#a71d5d">-</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;${{&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">!</span>contains(github.event.head_commit.message,&nbsp;<span style="color:#63a35c">'[skipci]'</span>)&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">&amp;</span><span style="color:#0086b3"></span><span style="color:#a71d5d">&amp;</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">!</span>contains(github.event.head_commit.message,&nbsp;<span style="color:#63a35c">'[ciskip]'</span>)</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}}</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;steps:</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">-</span>&nbsp;name:&nbsp;Run&nbsp;a&nbsp;multi<span style="color:#0086b3"></span><span style="color:#a71d5d">-</span>line&nbsp;script</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;uses:&nbsp;appleboy<span style="color:#0086b3"></span><span style="color:#a71d5d">/</span>ssh<span style="color:#0086b3"></span><span style="color:#a71d5d">-</span>action@master</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;with:</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;key:&nbsp;${{&nbsp;secrets.SSH_KEY&nbsp;}}</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;host:&nbsp;${{&nbsp;secrets.HOST&nbsp;}}</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;username:&nbsp;${{&nbsp;secrets.USER&nbsp;}}</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;port:&nbsp;${{&nbsp;secrets.PORT&nbsp;}}</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;script:&nbsp;<span style="color:#0086b3"></span><span style="color:#a71d5d">|</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;cd&nbsp;~<span style="color:#0086b3"></span><span style="color:#a71d5d">/</span>src;git&nbsp;pull&nbsp;origin&nbsp;dev</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#a71d5d">if</span>:&nbsp;always()</div></div><div style="text-align:right;margin-top:-13px;margin-right:5px;font-size:9px;font-style:italic"><a href="http://colorscripter.com/info#e" target="_blank" style="color:#e5e5e5text-decoration:none">Colored by Color Scripter</a></div></td><td style="vertical-align:bottom;padding:0 2px 4px 0"><a href="http://colorscripter.com/info#e" target="_blank" style="text-decoration:none;color:white"><span style="font-size:9px;word-break:normal;background-color:#e5e5e5;color:white;border-radius:10px;padding:1px">cs</span></a></td></tr></table></div>
<br>

위의 코드의 각 줄에 대해 살펴보자면 다음과 같다.<br>

1. 1번째 줄 : 위에서 설명했던 바와 같이 해당 workflow의 이름을 정의해줄 수 있다.
2. 3~5번째 줄 : dev branch에서 push가 발생했을 시 아래의 명령을 수행한다.
3. 10~13번째 줄 : commit 메시지에 [skipci] 혹은 [ciskip] 이 포함되어 있지 않을 경우만 아래의 명렁어가 실행된다 (!contains)
4. 16~ 번째 줄 : appleboy의 ssh-action 모듈을 활용해 with 줄 아래의 key, host, username, port로 접속해, 그 아래의 script를 실행한다. 나의 경우엔 단순히 git pull origin dev를 진행하는 것을 목표로 했다.
5. 25번째 줄 : 이번 코드엔 큰 영향이 없지만, 각 steps는 always() 혹은 failure() 를 갖게 되는데, 이에 따라 다른 내용들을 정의해줄 수도 있다. (ex: 성공시 slack post 등)

<br>
이제 코드가 잘 작동하는지 확인하기 위해, 해당 .yaml 파일을 커밋하고 실제로 push를 진행 해 보자.<br>

# 4. __실행 결과__
workflow가 성공했을 경우, 실패했을 경우, 스킵되었을 경우를 나누어서 살펴보면 아래와 같다.<br>

1. workflow가 성공했을 경우

![image4](https://drive.google.com/uc?id=1F7nv_16uHKl1mHNIYCyZHqIBTa1Cj6IX)

2. workflow가 실패했을 경우

![image5](https://drive.google.com/uc?id=1F5DYZ9WohmXQU2GeYuzZSoz9nXdmDJZv)

3. workflow가 스킵되었을 경우

![image6](https://drive.google.com/uc?id=1FN-OX6t2LFQOPDkAoeZ-vy3ZMnsu1k2H)

<br>

3에서 언급했다시피 위의 경우는 always(), success(), failure() 등으로 잡을 수 있는데, 이를 활용해 다양한 과정을 정의할 수 있다.<br>

# 5. __결론__
나의 경우엔 [build] 명령어가 포함되면 build를 진행하고, docker container들이 성공적으로 띄워지면 slack post를 보내는 등의 내용을 정의해서 사용하는 중이다.<br><br>
또한 위의 경우엔 cd, git pull 등의 명령어를 직접 입력했지만, alias를 활용하거나 sh 파일에 정의해 실행하는 등 상당히 다양한 커스터마이징이 가능하다.<br><br>
덧붙여 테스트 코드를 실행하는 과정까지 추가한다면, Jenkins 등의 툴 없이도 소규모 서버에서의 CI/CD는 충분히 가능할 정도라고 생각된다(개인 생각).
<br><br>
이처럼 Github Actions를 잘 활용하면 CI 과정이 상당히 편해질 수 있고, 각자의 입맛에 맞게 정의할 수 있다. 다들 편하게 할 수 있는 부분은 편하게 할 수 있는 개발자가 되길 바라며..