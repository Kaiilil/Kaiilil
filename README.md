#include <iostream>
#include <fstream>
#include <windows.h>
#include <conio.h>

using namespace std;
void Capphatdongmaze(int m,int n ,int **&maze){
    maze = new int *[m]; //
    for(int i = 0 ; i<m ;i++){
        maze[i] = new int[n];
    }
}
void Deletemaze(int **maze,int m ){
    for(int i = 0 ; i < m ; i++){
        delete[] maze [i];
    }
    delete[] maze ; 
}
void Capphatdongvisited(int m, int n, bool**& visited) {
	visited = new bool*[m];
	for (int i = 0; i < m; i++) {
		visited[i] = new bool[n];
	}
}
void Deletevisited(bool** visited, int m) {
	for (int i = 0; i < m; i++) {
		delete[] visited[i];
	}
	delete[] visited;
}
struct Robot {
	int x,y;
	int sum ;
	bool **visited; 
};
void Inputfile(int **&maze,int &m ,int &n ){
	ifstream file ; 
	file.open("C:/Users/DELL/Desktop/input.txt");
	file >> m >>n ; 
	Capphatdongmaze( m,n ,maze);
	for(int i = 0 ;i<m;i++){
		for(int j = 0 ; j <n ; j++){
			file >> maze[i][j]  ; 
		}
	}
	for(int i = 0 ; i < m ;i++){
		for(int j = 0 ; j < n;j++){
			cout << maze[i][j] <<"\t";
		}
		cout <<endl; 
		
	}
	file.close();
}
void Outputfile(Robot robot ,int **maze,int m ,int n,int *p,int size){
	int count = 0 ;
	for(int i = 0 ; i < m ;i++){
		for(int j = 0 ; j < n;j++){
			if(robot.visited[i][j]){
				count ++;
			}
		}
	}
	ofstream file("C:/Users/DELL/Desktop/output.txt");
	file << count <<endl; 
	for(int i = 0 ; i < size ;i++){
		file <<p[i]<<"\t";
		
	}
	file.close();
}
bool Checkmove(int x , int y,int m ,int n ){
	if(	x<0 ||y <0 || x>=m||y >=n){
		return false ; 
	}
	else{
		return true;
	}
}
void Visited(Robot &robot,int **maze,int m ,int n){
	
	for(int i = 0 ; i < m ; i++){
		for(int j = 0 ; j < n ; j++){
			robot.visited[i][j] = false ; 
		}
	}
}
void Findmax(Robot &robot, int **maze,int m ,int n,int *p,int &size,int &maxX,int &maxY){

	int max = -1 ; 
	cout << maze[robot.x][robot.y] << "\t";
	robot.sum +=maze[robot.x][robot.y];
	p[size] =maze[robot.x][robot.y];
	size ++;
	robot.visited[robot.x][robot.y] = true ; 
	if(Checkmove(robot.x-1,robot.y,m,n) && !robot.visited[robot.x-1][robot.y]){
		if(maze[robot.x-1][robot.y] >max){
			max = maze[robot.x-1][robot.y];
			maxX = robot.x-1; 
			maxY = robot.y; 
		}
	}
	if(Checkmove(robot.x+1,robot.y,m,n) && !robot.visited[robot.x+1][robot.y]){
		if(maze[robot.x+1][robot.y] >max){
			max = maze[robot.x+1][robot.y];
			maxX = robot.x+1; 
			maxY = robot.y; 
		}
	}
	if(Checkmove(robot.x,robot.y-1,m,n) && !robot.visited[robot.x][robot.y-1]){
		if(maze[robot.x][robot.y-1] >max){
			max = maze[robot.x][robot.y-1];
			maxX = robot.x; 
			maxY = robot.y-1; 
		}
	}
	if(Checkmove(robot.x,robot.y+1,m,n) && !robot.visited[robot.x][robot.y+1]){
		if(maze[robot.x][robot.y+1] >max){
			max = maze[robot.x][robot.y+1];
			maxX = robot.x; 
			maxY = robot.y+1; 
		}
	}
	if(max==-1){
		return ;
	}
	

	robot.x = maxX;
	robot.y = maxY ;
	
	return Findmax(robot,maze,m ,n,p,size,maxX,maxY);
}                                           
void Compare(Robot& robot1,Robot& robot2){
	if(robot1.sum == robot2.sum ){
		cout <<"HOA NHAU "  ;
	}
	else if(robot1.sum > robot2.sum	){
		cout <<"Robot 1 chien thang " ; 
	}
	else {
		cout <<"Robot 2 chien thang " ; 
	}
}
void TwoPaths(int *path1, int a,int *path2,int b,int m,int n, int **maze){
	cout <<"Cac toa do cua 2 Robot trung nhau :"  << endl ;
	for(int i = 0 ; i < m ; i++){
		for(int j = 0; j <n ; j++){
			for(int k = 0 ; k < a ; k++){
				for(int l = 0 ; l < b; l++){
					if(maze[i][j] == path1[k] && maze[i][j] ==path2[l]){
						cout <<"(" <<i <<"," <<j <<")"  <<"\t" ; 
					}
				}
			}
		}
	
	}
}
void init(Robot &robot1, Robot &robot2, int n, int m) {
	 for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            robot1.visited[i][j] = false;
        }
    }
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            robot2.visited[i][j] = false;
        }
    }
    // Khởi tạo thông tin cho robot 1
     cout << "Nhap vi tri xuat phat Robot1(x, y): "<< endl ; 
    cout << "Toa do X  :" ;
   	cin >> robot1.x ;
	cout << "Toa do Y :"  ;
	cin >> robot1.y;
    robot1.sum = 0;
   

    // Khởi tạo thông tin cho robot 2
     cout << "Nhap vi tri xuat phat Robot2(x, y): "<< endl ; 
    cout << "Toa do X  :" ;
   	cin >> robot2.x ;
	cout << "Toa do Y :"  ;
	cin >> robot2.y;
    robot2.sum = 0;
    
}

bool inRange(int x1, int y1, int x2, int y2, int n, int m) {
    return x1 >= 0 && x1 < n && y1 >= 0 && y1 < m && (x1 != x2 || y1 != y2);
}

bool moveRobot(int **maze, Robot &robot1, Robot robot2, int n, int m,int *p,int &size) {
    int x = robot1.x;
    int y = robot1.y;
    robot1.sum += maze[robot1.x][robot1.y];
    p[size]=maze[robot1.x][robot1.y];
    size++;
    robot1.visited[robot1.x][robot1.y] = true;
    robot2.visited[robot1.x][robot1.y] = true;
    int val = -1;
   
	robot1.visited[robot2.x][robot2.y] = true;
    robot2.visited[robot2.x][robot2.y] = true;
    // Di chuyển robot đến ô có giá trị lớn nhất
    if (inRange(x-1, y, robot2.x, robot2.y, n, m) && val < maze[x-1][y] && !robot1.visited[x-1][y]) {
		val = maze[x-1][y];
        robot1.x = x-1;
        robot1.y = y;
    }
    if (inRange(x+1, y, robot2.x, robot2.y, n, m) && val < maze[x+1][y] && !robot1.visited[x+1][y]) {
		val = maze[x+1][y];
        robot1.x = x+1;
        robot1.y = y;
    } 
    if (inRange(x, y-1, robot2.x, robot2.y, n, m) && val < maze[x][y-1] && !robot1.visited[x][y-1]) {
		val = maze[x][y-1];
        robot1.x = x;
        robot1.y = y-1;
    } 
    if (inRange(x, y+1, robot2.x, robot2.y, n, m) && val < maze[x][y+1] && !robot1.visited[x][y+1]) {
		val = maze[x][y+1];
        robot1.x = x;
        robot1.y = y+1;
    }
    if (val == -1) return false;
    return true;
}
void printRobotPath(int *p, int size, int *p2, int size2, Robot robot1, Robot robot2) {
    cout << "Duong di Robot: " << endl;
     for(int i = 0; i < size; i++) {
		cout << p[i] <<"\t";
    }
    cout <<endl ;
    cout << "Tong diem: " << robot1.sum << endl;
   
    cout << "Duong di Robot: " << endl;
    for(int j = 0; j < size2; j++) {
		cout << p2[j] <<"\t";
    }
    cout <<endl;
    cout << "Tong diem: " << robot2.sum << endl;
}

void PVP(int** maze, int n, int m, int* p, int& size, int* p2, int& size2, Robot& robot1, Robot& robot2) {
    int k = 1;
    bool robot1Stopped = false;
    bool robot2Stopped = false;
    
    while (true) {
        if (k % 2 != 0) {
            if (!robot1Stopped) {
                if (!moveRobot(maze, robot1, robot2, n, m, p, size)) {
                    robot1Stopped = true;
                }
            }
        }
        else {
            if (!robot2Stopped) {
                if (!moveRobot(maze, robot2, robot1, n, m, p2, size2)) {
                    robot2Stopped = true;
                }
            }
        }
        
        if (robot1Stopped && robot2Stopped) {
            break;
        }
        
        k++;
    }

    printRobotPath(p, size, p2, size2, robot1, robot2);
}

void Choi1nguoi(){
	struct Robot robot1;
    int m, n;
    int maxX, maxY;
    int** maze = nullptr;
    int* p = nullptr;
    int size = 0;
    robot1.sum = 0;
    Inputfile(maze, m, n);
    p = new int[m * n];
	Capphatdongvisited(m,  n, robot1.visited);
    Visited(robot1, maze, m, n);
    cout << "Nhap vi tri xuat phat Robot1(x, y): "<< endl ; 
    cout << "Toa do X  :" ;
   	cin >> robot1.x ;
	cout << "Toa do Y :"  ;
	cin >> robot1.y;
    Findmax(robot1, maze, m, n, p, size, maxX, maxY);
    Outputfile(robot1, maze, m, n, p, size);
    cout << endl << endl << endl;
    cout << "Tong cac buoc di cua Robot1: " << robot1.sum << endl;
    delete[] p;
    Deletemaze(maze, m);
	 Deletevisited(robot1.visited, m);
}
void Choi2nguoi(){
	struct Robot robot1,robot2;
    int m, n;
    int maxX, maxY;
    int** maze = nullptr;
    int *p = nullptr;
	int *k =nullptr;
    int size = 0;
	int size2 = 0 ;
	robot1.sum = 0 ;
	robot2.sum = 0 ;
	Inputfile(maze, m, n);
	p = new int[m * n];
	Capphatdongvisited(m,  n, robot1.visited);
	Visited(robot1, maze, m, n);
	 cout << "Nhap vi tri xuat phat Robot1(x, y): "<< endl ; 
    cout << "Toa do X  :" ;
   	cin >> robot1.x ;
	cout << "Toa do Y :"  ;
	cin >> robot1.y;
	 cout << "Nhap vi tri xuat phat Robot2(x, y): "<< endl ; 
    cout << "Toa do X  :" ;
   	cin >> robot2.x ;
	cout << "Toa do Y :"  ;
	cin >> robot2.y;
	Findmax(robot1,maze,m ,n,p,size,maxX,maxY);
	Outputfile(robot1 ,maze,m , n, p,size );

	k = new int[m * n];
	Capphatdongvisited(m,  n, robot2.visited);
	Visited(robot2,maze,m ,n);
	Findmax(robot2,maze,m ,n,k,size2,maxX,maxY);
	cout << endl  <<"Tong cac buoc di cua Robot1 :"<<robot1.sum; 
	cout << endl  <<"Tong cac buoc di cua Robot2 :"<<robot2.sum;
	cout << endl ;                  
	Outputfile(robot2 ,maze,m , n, p, size);
	Compare(robot1,robot2) ;
	cout << endl ;
	TwoPaths(p, size,k,size2,m,n,maze);
	delete[] p;
	delete[] k;
    Deletemaze(maze, m);
	 Deletevisited(robot1.visited,  m);
	  Deletevisited(robot2.visited,  m);
}
void ChoiPVP(){
	struct Robot robot1,robot2;
	int m, n,size1 = 0 ,size2 = 0 ;
	int** maze = nullptr;
	int* p =nullptr;
	int* p2 = nullptr ; 
	Inputfile(maze,m ,n );
	Capphatdongvisited(m,  n, robot1.visited);
	Visited(robot1,maze,m ,n);

	Capphatdongvisited(m,  n, robot2.visited);
	Visited(robot2,maze,m ,n);
	p = new int[m * n];
	p2 = new int[m * n];
	init(robot1,robot2,m,n);
	PVP(maze,m, n,p,size1,p2,size2,robot1,robot2);
	Compare(robot1,robot2);
	delete[]p;
	delete[]p2;
	Deletemaze(maze, m);
	Deletevisited(robot1.visited,  m);
	Deletevisited(robot2.visited,  m);
}
void gotoxy(int x, int y) {
    COORD coord;
    coord.X = x;
    coord.Y = y;
    SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), coord);
}

void setColor(int color) {
    SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), color);
}

void textcolor(int x)
{
	HANDLE mau;
	mau = GetStdHandle(STD_OUTPUT_HANDLE);
	SetConsoleTextAttribute(mau, x);
}
void ShowCur(bool CursorVisibility)
{
	HANDLE handle = GetStdHandle(STD_OUTPUT_HANDLE);
	CONSOLE_CURSOR_INFO cursor = { 1, CursorVisibility };
	SetConsoleCursorInfo(handle, &cursor);
}
void khung(int x,int y,int w,int h,int color){
	setColor(color);
	if(h<=1 ||w <=1)
	return ; 
	gotoxy(x,y);
	cout << char(218);
	gotoxy(x+w,y);
	cout <<char(191);
	gotoxy(x,y+h);
	cout<<char(192);
	gotoxy(x+w,y+h);
	cout <<char(217);
	for(int ix = x; ix<= x+w ; ix++){
		gotoxy(ix,y);
		usleep(5000);
		cout <<char(196);
		gotoxy(ix,y+h);
		cout <<char(196);
	}
	for (int iy = y; iy <= y + h; iy++){
        gotoxy(x,iy);
        usleep(5000);
        cout << char(179);
        gotoxy(x + w ,iy);
        cout << char(179);
        
    }
    gotoxy(x,y);
	cout << char(218);
	gotoxy(x+w,y);
	cout <<char(191);
	gotoxy(x,y+h);
	cout<<char(192);
	gotoxy(x+w,y+h);
	cout <<char(217);
}
void drawrobot(){
	setColor(11);
	 gotoxy(25, 3);
   cout << " _____   ____  ____   ____ _______    _____          __  __ ______ " << endl;
    gotoxy(25, 4);
   cout << "|  __ \\ / __ \\|  _ \\ / __ \\__   __|  / ____|   /\\   |  \\/  |  ____|" << endl;
    gotoxy(25, 5);
   cout << "| |__) | |  | | |_) | |  | | | |    | |  __   /  \\  | \\  / | |__   " << endl;
    gotoxy(25, 6);
   cout << "|  _  /| |  | |  _ <| |  | | | |    | | |_ | / /\\ \\ | |\\/| |  __|  " << endl;
    gotoxy(25, 7);
    cout << "| | \\ \\| |__| | |_) | |__| | | |    | |__| |/ ____ \\| |  | | |____ " <<endl;
    gotoxy(25, 8);
   cout << "|_|  \\_\\ ____/|____/ \\____/  |_|    \\_____/_/     \\_\\_|  |_|______|" <<endl;
}
void box(int x,int y,int w,int h,int t_color,int b_color,string nd){
	textcolor(b_color);
	for(int iy = y; iy <= y+h-1;iy++){
	for(int ix = x; ix <= x+w-1;ix++){
		
			gotoxy(ix,iy) ; cout <<" "; 
		}
	}
	setColor(7);
	gotoxy(x+1,y+1) ; 
	cout << nd ; 
	textcolor(1);	
	setColor(t_color);
	if(h<=1 ||w <=1)
	return ; 
	for(int ix = x; ix<= x+w ; ix++){
		gotoxy(ix,y);
		cout <<char(196);
		gotoxy(ix,y+h);
		cout <<char(196);
	}
	for (int iy = y; iy <= y + h; iy++){
        gotoxy(x,iy);
        cout << char(179);
        gotoxy(x + w ,iy);
        cout << char(179);
        
    }
    gotoxy(x,y);
	cout << char(218);
	gotoxy(x+w,y);
	cout <<char(191);
	gotoxy(x,y+h);
	cout<<char(192);
	gotoxy(x+w,y+h);
	cout <<char(217);
}
void n_box(int x,int y,int w,int h, int t_color,int  b_color){
		box(x,y, w, h, t_color, b_color, "Choi voi may");
		box(x,y+2, w, h, t_color, b_color,"Choi voi nguoi");
		box(x,y+4, w, h,t_color, b_color, "PVP");
		box(x, y+6, w, h, t_color, b_color, "Quit");

		
		
			gotoxy(x , y+2);cout << char(195);
			gotoxy(x , y+4);cout << char(195);
			gotoxy(x , y+6);cout << char(195);
			
			gotoxy(x + w, y  );    cout << char(180);
			gotoxy(x + w, y+2);cout << char(180);
			gotoxy(x + w, y+4);cout << char(180);
			gotoxy(x + w, y+6);cout << char(180);
		
}
void Vungsang(int x,int y,int w,int h,int t_color){
	
	setColor(t_color);
	if(h<=1 ||w <=1)
	return ; 
	for(int ix = x; ix<= x+w ; ix++){
		gotoxy(ix,y);
		cout <<char(196);
		gotoxy(ix,y+h);
		cout <<char(196);
	}
	for (int iy = y; iy <= y + h; iy++){
        gotoxy(x,iy);
        cout << char(179);
        gotoxy(x + w ,iy);
        cout << char(179);
        
    }
    gotoxy(x,y);
	cout << char(218);
	gotoxy(x+w,y);
	cout <<char(191);
	gotoxy(x,y+h);
	cout<<char(192);
	gotoxy(x+w,y+h);
	cout <<char(217);
}
void menu(){
	int x = 50, y = 15;
	ShowCur(0);
	int w = 20;
	int h = 2;
	int t_color = 11;
	int b_color = 1;
	n_box(x,y,w,h,t_color,b_color);
	int xp = x ; int yp = y;
	int xcu = xp ; int ycu = yp;
//	-------------------------------------

	bool kt = true;
	int selected = 0;
	
	while (true)
	{
		//------ in ----
		if (kt == true)
		{
			//----- back space ----
			gotoxy(xcu, ycu);
			Vungsang(xcu, ycu, w, h,1);//rs thanh sang cu
			xcu = xp;ycu = yp;
			//-------------
			Vungsang(xp, yp, w, h,75);
			kt = false;
		}
		//----- dieu khien ---- //----- di chuyen ----
		if (_kbhit())
		{
			char c = _getch();
			if (c == -32)
			{
				kt = true;// đã bấm
				c = _getch();
				if (c == 72)
				{
					if(yp != y)yp -= 2;
					else
					{
						yp = y + 2*(4 - 1);
					}
				}
				else if (c == 80 )
				{
					if(yp != y + 2*(4 - 1))yp += 2;
					else
					{
						yp = y;
					}
				}
			}
			else if(c==13 &&yp == y){
				Vungsang(xcu, ycu, w, h,1);
				system("cls");

				gotoxy(0,0);
                Choi1nguoi();
			
				
                
			}
			else if(c==13 && yp == y+2){
				Vungsang(xcu, ycu, w, h,1);
				system("cls");

				gotoxy(0,0);
                Choi2nguoi();
			}
			else if (c==13 && yp == y+4){
				Vungsang(xcu, ycu, w, h,1);
				system("cls");

				gotoxy(0,0);
                ChoiPVP();
			}
			else if (c==13 && yp == y+6){
				exit(0) ; 
			}

		
	}
}
}
int main(){
   khung(0,0,118,28,2);
	drawrobot();
	ShowCur(0);
 	menu();
  return 0;
}
