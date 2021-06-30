#include <stdio.h>
#include <stdlib.h>
#include <cstring>
  
#define INP "input.inp"
#define OUT "output.out"
  
// đọc dữ liệu trong file input
int readData(int ***G, int *n, int *a, int *b) {
    FILE *fi = fopen(INP, "r");
    if (fi == NULL) {
        printf("file input not found!\n");
        return 0;
    }
    printf("start read file\n");
  
    fscanf(fi, "%d %d %d", n, a, b);
  
    *G = (int **) malloc((*n) * sizeof(int));
    for (int i = 0; i < *n; i++) {
        (*G)[i] = (int *) malloc((*n) * sizeof(int));
        for (int j = 0; j < *n; j++) {
            int x;
            fscanf(fi, "%d", &x);
            (*G)[i][j] = x;
        }
    }
  
    fclose(fi);
    printf("done read file\n");
    return 1;
}
  
// thuật toán dijkstra
int dijkstra(int **G, int n, int a, int b, int P[]) {
  
    /* Do mảng tính từ G[0][0] nên cần giảm vị trí 
     đi 1 đơn vị để tính toán cho phù hợp*/
    a--;
    b--;
  
    printf("start dijkstra\n");
  
    int* Len = (int *) malloc(n * sizeof(int));
    int* S = (int *) malloc(n * sizeof(int));
  
    int sum = 0;            // giá trị vô cùng
  
    // tính giá trị vô cùng (sum)
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            sum += G[i][j];
        }
    }
  
    // đặt vô cùng cho tất cả cặp cạnh không nối với nhau
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (i != j && G[i][j] == 0)
                G[i][j] = sum;
        }
    }
  
    for (int i = 0; i < n; i++) {
        Len[i] = sum;       // khởi tạo độ dài từ a tới đỉnh là vô cùng
        S[i] = 0;           // danh sách các điểm đã xét
        P[i] = a;           // đặt điểm bắt đầu của mỗi điểm là a
    }
  
    Len[a] = 0;             // đặt độ dài từ a -> a là 0
  
    int i;
    while (S[b] == 0) {                 // trong khi điểm cuối chưa được xét
        for (i = 0; i < n; i++)          // tìm một vị trí mà không phải là vô cùng
            if (!S[i] && Len[i] < sum)
                break;
  
        // i >=n tuc la duyet het cac dinh ma khong the tim thay dinh b -> thoat
        if (i >= n) {
            printf("done dijkstra\n");
            return 0;
        }
  
        for (int j = 0; j < n; j++) {    // tìm điểm có vị trí mà độ dài là min
            if (!S[j] && Len[i] > Len[j])
                i = j;
        }
  
        S[i] = 1;                       // cho i vào danh sách xét rồi
  
        for (int j = 0; j < n; j++) {    // tính lại độ dài của các điểm chưa xét
            if (!S[j] && Len[i] + G[i][j] < Len[j]) {
                Len[j] = Len[i] + G[i][j];      // thay đổi Len
                P[j] = i;                       // đánh dấu điểm trước nó
            }
        }
    }
    printf("done dijkstra\n");
    return Len[b];
}
  
//  truy vết đường đi
void back(int a, int b, int *P, int n, char *path) {
  
    //char *path = (char *) malloc((n * 10) * sizeof(char));
  
    /* Do mảng tính từ G[0][0] nên cần giảm vị trí 
     Đi 1 đơn vị để tính toán cho phù hợp */
    a--;
    b--;
  
    printf("start find path\n");
  
    int i = b;
    int point[n];   // danh sách các đỉnh của đường đi
    int count = 0;
  
    /* Do ta đang tính toán từ đỉnh 0 nên 
     muốn hiển thị từ đỉnh 1 thì cần dùng i +1 để phù hợp */
  
    point[count++] = i + 1;
    while (i != a) {
        i = P[i];
        point[count++] = i + 1;
    }
  
    strcpy(path, "");
    char temp[10];
    for (i = count - 1; i >= 0; i--) {
        sprintf(temp, "%d", point[i]);
        strcat(path, temp);
  
        if (i > 0) {
            sprintf(temp, " --> ");
            strcat(path, temp);
        }
    }
  
    printf("done find path\n");
}
  
void outResult(int len, char* path) {
    FILE *fo = fopen(OUT, "w");
  
    if (len > 0) {
        fprintf(fo, "\nLength of %c to %c is %d\n", path[0],
                path[strlen(path) - 1], len);
    }
  
    fprintf(fo, "path: %s\n", path);
  
    fclose(fo);
}
  
int main() {
    int **G, n, a, b, len;
  
    if (readData(&G, &n, &a, &b) == 0) {
        return 0;
    }
  
    char *path = (char *) malloc((10 * n) * sizeof(char));
    int P[n];
  
    len = dijkstra(G, n, a, b, P);
  
    if (len > 0) {
        back(a, b, P, n, path);
        outResult(len, path);
    } else {
        char *path = (char *) malloc((n * 10) * sizeof(char));
        sprintf(path, "Không có đường đi từ %d đến %d\n", a, b);
        outResult(len, path);
    }
  
    printf("done - open file output to see result\n");
    return 0;
}




