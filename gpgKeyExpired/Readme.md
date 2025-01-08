## gpgKey Expired 에러

### 상황
```
~$ sudo apt update
기존:1 https://download.docker.com/linux/ubuntu focal InRelease
기존:2 https://deb.nodesource.com/node_16.x focal InRelease                                                                                                                                                                 
기존:3 https://dl.google.com/linux/chrome/deb stable InRelease                                                                                                                                                              
기존:5 http://archive.ubuntu.com/ubuntu focal InRelease                                                                                                                                                                     
기존:6 http://security.ubuntu.com/ubuntu focal-security InRelease                                                                                                                                                      
기존:7 https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 InRelease                                                                                
받기:9 https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 InRelease [4,009 B]                                
받기:10 https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4/multiverse amd64 Packages [90.0 kB]
받기:11 https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4/multiverse arm64 Packages [80.3 kB] 
받기:4 https://packages.gitlab.com/gitlab/gitlab-ce/ubuntu focal InRelease [23.3 kB]
오류:4 https://packages.gitlab.com/gitlab/gitlab-ce/ubuntu focal InRelease
  다음 서명이 올바르지 않습니다: EXPKEYSIG 3F01618A51312F3F GitLab B.V. (package repository signing key) <packages@gitlab.com>
받기:8 https://packages.gitlab.com/runner/gitlab-runner/ubuntu focal InRelease [23.6 kB]
오류:8 https://packages.gitlab.com/runner/gitlab-runner/ubuntu focal InRelease
  다음 서명이 올바르지 않습니다: EXPKEYSIG 3F01618A51312F3F GitLab B.V. (package repository signing key) <packages@gitlab.com>
패키지 목록을 읽는 중입니다... 완료
W: GPG 오류: https://packages.gitlab.com/gitlab/gitlab-ce/ubuntu focal InRelease: 다음 서명이 올바르지 않습니다: EXPKEYSIG 3F01618A51312F3F GitLab B.V. (package repository signing key) <packages@gitlab.com>
E: The repository 'https://packages.gitlab.com/gitlab/gitlab-ce/ubuntu focal InRelease' is not signed.
N: Updating from such a repository can't be done securely, and is therefore disabled by default.
N: See apt-secure(8) manpage for repository creation and user configuration details.
W: GPG 오류: https://packages.gitlab.com/runner/gitlab-runner/ubuntu focal InRelease: 다음 서명이 올바르지 않습니다: EXPKEYSIG 3F01618A51312F3F GitLab B.V. (package repository signing key) <packages@gitlab.com>
E: The repository 'https://packages.gitlab.com/runner/gitlab-runner/ubuntu focal InRelease' is not signed.
N: Updating from such a repository can't be done securely, and is therefore disabled by default.
N: See apt-secure(8) manpage for repository creation and user configuration details.
```

```
W: GPG 오류: https://packages.gitlab.com/gitlab/gitlab-ce/ubuntu focal InRelease: 다음 서명이 올바르지 않습니다: EXPKEYSIG 3F01618A51312F3F GitLab B.V. (package repository signing key) <packages@gitlab.com>
E: The repository 'https://packages.gitlab.com/gitlab/gitlab-ce/ubuntu focal InRelease' is not signed.

W: GPG 오류: https://packages.gitlab.com/runner/gitlab-runner/ubuntu focal InRelease: 다음 서명이 올바르지 않습니다: EXPKEYSIG 3F01618A51312F3F GitLab B.V. (package repository signing key) <packages@gitlab.com>
E: The repository 'https://packages.gitlab.com/runner/gitlab-runner/ubuntu focal InRelease' is not signed.
```

깃랩 러너와 깃랩CE 의 GPG KEY 가 만료됨

## 해결법

### gitlab runner 의 경우
```
curl -fsSL "https://packages.gitlab.com/runner/gitlab-runner/gpgkey" | sudo gpg --dearmor -o /usr/share/keyrings/runner_gitlab-runner-archive-keyring.gpg
```

새로 받은 gpgkey 를 /usr/share/keyrings의 gpg 로 교체한다

### gitlabCE 의 경우
```
$ curl -fsSL https://packages.gitlab.com/gitlab/gitlab-ce/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/gitlab_gitlab-ce-archive-keyring.gpg

File '/usr/share/keyrings/gitlab_gitlab-ce-archive-keyring.gpg' exists. Overwrite? (y/N) y
```
```
$ sudo apt update
기존:1 https://download.docker.com/linux/ubuntu focal InRelease
기존:2 https://dl.google.com/linux/chrome/deb stable InRelease                                                                                                                                                                     
기존:3 https://deb.nodesource.com/node_16.x focal InRelease                                                                                                                                                                        
기존:5 https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 InRelease                                                                                                                                                         
기존:6 http://archive.ubuntu.com/ubuntu focal InRelease                                                                                                   
기존:7 http://security.ubuntu.com/ubuntu focal-security InRelease                                                                                         
기존:9 https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 InRelease                                                                                
기존:8 https://packages.gitlab.com/runner/gitlab-runner/ubuntu focal InRelease
받기:4 https://packages.gitlab.com/gitlab/gitlab-ce/ubuntu focal InRelease [23.3 kB]
받기:10 https://packages.gitlab.com/gitlab/gitlab-ce/ubuntu focal/main amd64 Packages [70.9 kB]
내려받기 94.2 k바이트, 소요시간 3초 (27.2 k바이트/초)
패키지 목록을 읽는 중입니다... 완료
의존성 트리를 만드는 중입니다       
상태 정보를 읽는 중입니다... 완료
19 패키지를 업그레이드할 수 있습니다. 확인하려면 'apt list --upgradable'를 실행하십시오.
```