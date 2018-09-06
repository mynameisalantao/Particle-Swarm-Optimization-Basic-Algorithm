Particle-Swarm-Optimization
=============================
以鳥類尋覓食物為例:
每隻鳥的行動會同時受到自己與群體影響
1.受到自己的影響:
前一刻自己的飛行速度
swarm(i,5)與swarm(i,6)
先前找過離食物最近的方向
swarm(i,3)-swarm(i,1)與swarm(i,4)-swarm(i,2)
2.受到群體的影響:
先前大家找過離食物最近的方向
swarm(gbest,3)-swarm(i,1)與swarm(gbest,4)-swarm(i,2)

<pre><code>
首先定義陣列中每個位置參數的意義
第1行:u的位置
第2行:v的位置
第3行:目前最佳u的位置
第4行:目前最佳v的位置
第5行:u的速度
第6行:v的速度
第7行:目前最佳適應值

%% 初始化粒子位置、速度與適應值
swarm=zeros(swarms,7);   %先配置足夠的矩陣空間來儲存每個粒子的資訊

%將每個粒子的(u,v)初始位置完全採用亂數[0,50]
for i=1:swarms
    for j=1:2
        swarm(i,j)=rand()*50;
        %先將目前最佳(u,v)值預設為初始(u,v)值
        swarm(i,j+2)=swarm(i,j);
    end
end

swarm(:,7)=10000;                       % 初始的適應值
swarm(:,5)=0;                           % 初始的u方向速度
swarm(:,6)=0;                           % 初始的v方向速度

</pre></code>
