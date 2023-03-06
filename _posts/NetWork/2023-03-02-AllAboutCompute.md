---
title: Booting
excerpt: Abstract of the booting process

toc: true
toc_label: About
toc_sticky: true

categories: network

date: 2023-03-02
last_modified_at: 2023-03-02
---
<br>
# All About the Computer
컴퓨터 전원 컴퓨터를 키면 몇 개의 검은색 화면을 몇 번 지나치고 운영 체제(Windows 기준)가 켜진다.<br>
이번 포스트에서는 운영체제가 켜지는 과정에서 사용되는 모든 영어를 설명하고자 한다.<br>
<br>

# CPU 초기화
CPU가 가지고 있는 값들을 모두 지우고 CPU 레지스터인 PC - Programmer Counter 값을 모두 지운다.<br>
이때 PC 값을 BIOS의 주소로 설정하고 CPU가 이상이 있는지 없는지를 검사한다.<br>
<br>

# 부팅
부팅의 시작은 Boot Manager가 메모리에 로드되면서 부터이다.<br>
메인보드에 설치되어 있는 펌웨어 타입에 따라 Boot Manager 실행에 차이가 있다.<br>
차이에 따라 BIOS 또는 EFI라고 불린다.<br>
<br>

## BIOS - Basic Input Output System
### POST - Power On Self Test
컴퓨터에 전원이 공급되었을 때, 컴퓨터 키보드, 램, 디스크 드라이브 그리고 기타 하드웨어 등이 바르게 동작하는지를 확인하는 과정.<br>
필요한 하드웨어들이 모두 감지되고, 또한 적절하게 동작되고 있으면, 컴퓨터는 부팅을 시작한다.<br>
<br>

### MBR - Master Boot Record
BIOS 는 부팅 섹터를 찾기 위해 부팅 가능한 디바이스 목록을 검색한다.<br> 
부팅 가능한 장치가 하드 드스크라면 이 장치의 부트 섹터를 MBR(Master Boot Record) 라고 부른다.<br>
<br>
MBR은 MBR Boot Code와 파티션 테이블로 구성된다.<br>
하드 디스크의 파티션 정보는 MBR의 파티션 테이블에 존재하며 하드 디스크의 첫 섹터에 있다.<br>
MBR boot code가 실행되면서 파티션 테이블 중 활성화 파티션(부팅 가능한 파티션)을 찾아 메모리에 로드한다.<br>
부팅 가능한 파티션은 각 파티션의 첫 번째 바이트로 알 수 있다. -> 0x80: 부팅가능 / 0x00: 부팅 불가<br>
이때 부팅 가능한 파티션을 VBR(Volume Boot Record)라고 한다.<br>
<br>

### VBR - Volume Boot Record
VBR에도 VBR Boot code가 있고 해당 코드는 자신의 파티션에서 boot manager program을 찾는다.<br>
Boot manager가 boot manager program을 메모리에 로드하면 부팅이 시작된다.<br>

### MBR / VBR 이 Boot manager을 찾는 이유
MBR/VBR의 최대 용량은 512바이트 뿐이므로 도스 같은 운영체제 정도에는 적합함<br>
그러나 MBR/VBR의 최대 용량은 512바이트 뿐이므로 윈도우 같은 복잡한 운영체제의 커널을 로드 할 준비를 하기에는 용량이 턱없이 모자람<br>
<br>

## EFI - Extensible Firmware Interface
### POST - Power On Self Test
BIOS와 마찬가지로 POST 동작을 수행한다.<br>
<br>

### Boot Code & Boot Manager
Boot Code는 MBR이나 VBR이 존재하지 않고 펌웨어 안에 존재한다.<br>
즉 POST과정을 마친후 바로 boot code가 boot manager위치를 찾아 boot load가 진행되는 것이다.<br>
<br>

## Boot Manager
**EFI 방식이나 BIOS 방식 둘다 최종적으로 Boot Manager(bootmgr) 를 메모리에 로드하여 실행시킨다.**<br>
Boot Manager 는 BCD(Boot Configuration Data) 데이터를 읽어 시스템을 시작한다.<br> 
Windows OS인 경우 Windows Boot Loader 인 winload.exe 를 로드하고 실행한다.<br>
<br>

## Boot Loader
OS가 시동되기 이전에 미리 실행되면서 커널이 올바르게 시동되기 위해 필요한 모든 관련 작업을 마무리하고 최종적으로 운영 체제를 시동시키기 위한 목적을 가진 프로그램을 말한다.<br>
