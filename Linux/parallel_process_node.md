# node에 뿌리고 확인하는 명령어

```
qstat -f: 돌리고 있는 노드 현황 확인
cat test.sh.o.58912.1: 해당 process의 output 확인
cat test.sh.e.58912.1: 해당 process의 error 확인

```

<br>
<br>

# node에 뿌리는 명령어

```
qsub -q all.q@avm-node002,all.q@avm-node003,all.q@avm-node004,all.q@avm-node005,all.q@avm-node006,all.q@avm-node007,all.q@avm-node008,all.q@avm-node009,all.q@avm-node010,all.q@avm-node011,all.q@avm-node012,all.q@avm-node013,all.q@avm-node014,all.q@avm-node015,all.q@avm-node016 -t 1-$(wc -l < list) -tc 80 test.sh
```
