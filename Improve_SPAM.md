# Improve SPAM

## Origem

A Qustão foi aplicada na Final Brasileira de Programação 2019 que ocorreu em Campina Grande. 
A maratona teve um espelho no juiz online URI que pode ser acessado no [link](https://www.urionlinejudge.com.br/judge/pt/contests/view/496).

O problema era a questão I da prova e foi hospedada no URI no [link](https://www.urionlinejudge.com.br/judge/pt/problems/view/3020)


## Enunciado 

  After the amazing job you did cleaning up duplicate users from the client database, your boss is eager to be impressed by your improvements to the company SPAM (System for Publishing Amazing Marketing).
Despite the marketing campaigns being extremely useful for clients, some complaints were received by the customer service indicating that too many messages are being sent, and that certain clients even receive the same message multiple times.
SPAM is based on mailing lists. Each mailing list is composed of client emails and/or other mailing lists. Client emails might be added to existing mailing lists at any point in time, while only when a mailing list is created it can be added to any number of existing mailing lists. Notice that it is not possible to create several mailing lists at the same time.

  When a message is sent to a mailing list, the system sends the message to each address in the list. If the address in the list is a client email, then the message is sent to that client email; if instead the address is a mailing list, then the process is started for that mailing list.
Due to privacy reasons, in the following example mailing lists and client emails are repre-sented by integers. Suppose that 1, 2 and 3 are mailing lists, while 4 and 5 are client emails.Moreover, mailing list 1 contains mailing lists 2 and 3, mailing list 2 contains client emails 4 and 5, while mailing list 3 contains client email 4 and mailing list 2. Now suppose that a message is sent to mailing list 1. This means that the list is processed as described above, and then mailing lists 2 and 3 are also processed. When mailing list 2 is processed, the message is sent to client emails 4 and 5. When mailing list 3 is processed, a second message is sent to
client email 4, and mailing list 2 is processed again, which yields a third message sent to client email 4 and a second message sent to client email 5. Thus, a total of five messages are sent to client emails.
Your task is to optimize SPAM in such a way that no client receives the same message multiple times. As a first step, your boss wants to know the number of messages sent before and after your improvements. In the above example, just two messages should be sent to client emails after your work is done.
 
 
### Input

The first line contains two integers N and L (2 ≤ N ≤ 2000, 1 ≤ L ≤ min(N − 1, 1000)), representing respectively the number of addresses in the system, and the number of addresses that are mailing lists. Addresses are identified by distinct integers from 1 to N . Addresses from 1 to L are mailing lists, while the rest are client emails. For i = 1, 2, . . . , L, the i-th of the next L lines describes mailing list i with an integer K (1 ≤ K < N ) followed by K different integers M 1 , M 2 , . . . , M K (1 ≤ M i ≤ N for i = 1, 2, . . . , K), indicating that the mailing list contains the K addresses M 1 , M 2 , . . . , M K . Each client address appears in at least one mailing list.

### Output 

Output a single line with two integers B and A indicating respectively the number of messages sent to client emails before and after your improvements, if a message is sent to mailing list 1. Because these numbers can be very large, output the remainder of dividing them by 109 + 7.

## Resolução

Para resolução desse problema é necessário abrir todas as árvores de possibilidades e contar quantos nós sem filhos existem em toda a árvore.
Logicamente isso se torna impossível de fazer no tempo limite de 2 segundos, pois no pior caso o número de filhos pode ultrapassar a casa dos bilhoes.

Para melhorar a complexidade do problema, assim como o problema de fibonacci, é necessário realizar a memorização dos nós já visitados, não somente com um booleano mas também
com o valor associado a quantos nós sem filhos o nó X tem.

Um ponto a lembrar é que como a resposta pode ser muito grande os calculos devem ser feitos em mod 1E9+7.

## Código

```c++
#include<bits/stdc++.h>
using namespace std;
using ll = long long;
const int MAXN = 2010;
const ll mod = 1000000007;
ll dp[MAXN];
vector<vector<int>> vs;

ll visit(int u, int n, int l){
	if(dp[u]!=-1LL) 
		return dp[u];
	if(u>l) dp[u]=1LL;
	else{
		dp[u]=0LL;
		for(auto x:vs[u-1])
			dp[u]= (dp[u]+visit(x, n, l))%mod;
	}
	return dp[u];
}
int32_t main(){
	int n, l;
	cin >> n >> l;
	for(int i=0; i<MAXN; ++i) dp[i]=-1LL;
	
	vs.assign(l, vector<int>());
	for(int i=0; i<l; ++i){
		int k;
		cin >> k;
		vs[i].assign(k, 0);
		for(auto &x:vs[i]) cin >> x;
	}
	ll total = visit(1, n, l);
	
	ll ans = 0LL;
	for(int i=l+1; i<=n; ++i)
		if(dp[i]!=-1LL) ans+=dp[i];
	cout << total << " " << ans << endl;
}
```
