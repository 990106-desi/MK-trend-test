[filename, filepath]=uigetfile('MK.xls');
full_filepath=[filepath filename];
[X,TXTX,RAWX]=xlsread(full_filepath,1);
x=X(:,1);
y=X(:,2);
n=size(y,1);
Sk=zeros(size(y));
UFk=zeros(size(y));
s1=0;
for i=2:n
    for j=1:i
        if y(i)>y(j)
            s1=s1+1;
        else
            s1=s1+0;
        end
    end
    Sk(i)=s1;
    E=i*(i-1)/4; %average
    Var=i*(i-1)*(2*i+5)/72;%variance
    UFk(i)=(Sk(i)-E)/sqrt(Var);
end
%
y2=zeros(size(y));
Sk2=zeros(size(y));
UBk=zeros(size(y));
s2=0;
for i=1:n
    y2(i)=y(n-i+1);%
end
for i=2:n
    for j=1:i
        if y2(i)>y2(j)
            s2=s2+1;
        else
            s2=s2+0;
        end
    end
    SK2(i)=s2;
    E=i*(i-1)/4;%
    Var=i*(i-1)*(2*i+5)/72;%
    UBk(i)=-(SK2(i)-E)/sqrt(Var);
end
UBk2=zeros(size(y));
for i=1:n
    UBk2(i)=UBk(n-i+1);%
end
%linear regression
x1=x-x(1)+1;
r=corrcoef(x1,y); %Correlation coefficient
R2=r(1,2)^2;
C=polyfit(x1,y,1); %
%
dFB=max(max(UFk)-min(UFk),max(UBk2)-min(UBk2));
dFB1=min(min(UFk),min(UBk2))-0.2*dFB;
dFB2=max(max(UFk),max(UBk2))+0.2*dFB;
alpha1=0.05;%
alpha2=0.01;%
PZ1=norminv(1-alpha1/2,0,1);
PZ2=norminv(1-alpha2/2,0,1);
if dFB1>-PZ2
    dFB1=-5;
end
if dFB2<PZ2
    dFB2=5;
end
figure
plot(x,UFk,'r-','linewidth',0.8);
hold on
plot(x,UBk2,'b-','linewidth',0.8);
plot(x,PZ1*ones(n,1),'k--','linewidth',0.8);
plot(x,-PZ1*ones(n,1),'k--','linewidth',0.8);
%plot(x,PZ2*ones(n,1),'g--');
plot(x,0*ones(n,1),'k-','linewidth',1);
axis([min(x),max(x),-6,4]);
h=legend('UF','UB','0.05 ');
xlabel('Year','FontName','TimesNewRoman','FontSize',18);
ylabel('Statistics','FontName','TimesNewRoman','Fontsize',18);
% title('Mann-Kendall','FontName','TimesNewRoman','Fontsize',22) ;
set(h,'box','off');
legend('location','north');
legend('orientation','horizontal')
%h=legend('UF','UB','0.05');
% xlabel('Year','FontName','TimesNewRoman','FontSize',18);
% ylabel('Statistics','FontName','TimesNewRoman','Fontsize',18);
% title('Mann-Kendall   All year','FontName','TimesNewRoman','Fontsize',22) ;
% set(h,'box','off');
% legend('location','north');
% legend('orientation','horizontal')

% %plot(x,-PZ1*ones(n,1),'k-','linewidth',2);
% %plot(x,-PZ2*ones(n,1),'g-','linewidth',2);
% hold off
% output='M-K statistical curve graph';
% saveas(gcf,output,'jpg')
% [filename filepath]=uigetfile('test.xls');
% full_filepath=[filepath filename];
% xlswrite(full_filepath,x,'Sheet1','A1');
% xlswrite(full_filepath,UFk,'Sheet1''B1');
% xlswrite(full_filepath,UBk2,'Sheet1','C1');
