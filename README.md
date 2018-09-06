Particle-Swarm-Optimization
=============================
定義變數

<pre><code>
%% 初始化參數
iterations=30; %要進行迭代的次數
inertia=1.0;   %慣性(會與前一次的速度相乘)
correction_factor=1.0;   %粒子個體與群體最佳解的權重
swarms=100;   %總粒子數量
</pre></code>

定義陣列中每個位置參數的意義<br/>
第1行:u的位置<br/>
第2行:v的位置<br/>
第3行:目前最佳u的位置<br/>
第4行:目前最佳v的位置<br/>
第5行:u的速度<br/>
第6行:v的速度<br/>
第7行:目前最佳適應值<br/>

<pre><code>
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

接著進入迭代部分<br/>
此處的適應函數為f(u,v)=(u-30)^2+(v-30)^2，不斷帶入適應解(u,v)來得到更小的適應值f(u,v) <br/>
此式其實就是點(u,v)到(30,30)的距離平方<br/>
要使適應值越來越小，(u,v)勢必會越來越接近(30,30) <br/>
每當找到更小的適應值，該粒子便會把這個(u,v)與f(u,v)分別設為目前最佳適應解u、目前最佳適應解v和目前最佳適應值<br/>
並在每次迭代時找到所有粒子當中最小的最佳適應值作為全域目前最佳適應值，當作日後群體的最佳解位置<br/>


<pre><code>
%每個粒子依序進行位置更新
    for i = 1 : swarms
        swarm(i,1)=swarm(i,1)+swarm(i,5)/1.2;     % 更新u方向的位置(加上目前u方向的速度)
        swarm(i,2)=swarm(i,2)+swarm(i,6)/1.2;     % 更新v方向的位置(加上目前v方向的速度)
        u=swarm(i,1);
        v=swarm(i,2);
        value=(u-30)^2+(v-30)^2;                  % 適應函數
        % 如果目前的粒子(u,v)帶入適應函數得到比目前最佳適應值更小(佳)的適應值，則更新目前最佳適應解與目前最佳適應值
        if value < swarm(i,7)                     
            swarm(i,3)=swarm(i,1);                % 更新目前最佳適應解u
            swarm(i,4)=swarm(i,2);                % 更新目前最佳適應解v
            swarm(i,7)=value;                     % value為目前最佳適應值
        end
    end
    [temp,gbest]=min(swarm(:,7));                 % 從每個粒子的目前最佳適應值中，找出全域目前最佳適應值
</pre></code>



以鳥類尋覓食物為例:<br/>
每隻鳥的行動會同時受到自己與群體影響<br/>
1.受到自己的影響:<br/>
前一刻自己的飛行速度<br/>
swarm(i,5)與swarm(i,6) <br/>
先前找過離食物最近的方向<br/>
swarm(i,3)-swarm(i,1)與swarm(i,4)-swarm(i,2) <br/>
2.受到群體的影響:<br/>
先前大家找過離食物最近的方向<br/>
swarm(gbest,3)-swarm(i,1)與swarm(gbest,4)-swarm(i,2) <br/>

粒子的速度更新的算法如下:
<pre><code>
%根據每個粒子的目前位置、目前速度、目前最佳適應解與全域目前最佳適應值，來計算下一刻粒子的速度
    for i= 1:swarms
        swarm(i,5)=rand*inertia*swarm(i,5)+correction_factor*rand*(swarm(i,3)-swarm(i,1))+correction_factor*rand*(swarm(gbest,3)-swarm(i,1));   % 下一刻粒子u方向的速度
        swarm(i,6)=rand*inertia*swarm(i,6)+correction_factor*rand*(swarm(i,4)-swarm(i,2))+correction_factor*rand*(swarm(gbest,4)-swarm(i,2));   % 下一刻粒子v方向的速度
    end
</pre></code>
