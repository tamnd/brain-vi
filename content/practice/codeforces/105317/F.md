---
title: "CF 105317F - Người chạy mê cung"
description: "Chúng ta có một lưới rất nhỏ, nhiều nhất là 10 x 10, trong đó mỗi ô đều chứa một chữ cái viết thường. Từ bất kỳ ô nào, chúng ta được phép di chuyển đến bất kỳ ô nào trong số 8 ô lân cận của nó, kể cả các đường chéo và chúng ta được phép xem lại các ô tùy ý nhiều lần."
date: "2026-06-23T06:07:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105317
codeforces_index: "F"
codeforces_contest_name: "JPC 1.0"
rating: 0
weight: 105317
solve_time_s: 57
verified: true
draft: false
---

[CF 105317F - Người chạy mê cung](https://codeforces.com/problemset/problem/105317/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới rất nhỏ, nhiều nhất là 10 x 10, trong đó mỗi ô đều chứa một chữ cái viết thường. Từ bất kỳ ô nào, chúng ta được phép di chuyển đến bất kỳ ô nào trong số 8 ô lân cận của nó, kể cả các đường chéo và chúng ta được phép xem lại các ô tùy ý nhiều lần. Hạn chế duy nhất đối với việc di chuyển là một bước phải thực sự thay đổi ô, vì vậy không được phép giữ nguyên vị trí. 

Mỗi truy vấn đưa ra một chuỗi ngắn và chúng ta cần quyết định xem có tồn tại một bước đi trên lưới sao cho chuỗi các ký tự ô được truy cập đánh vần chính xác chuỗi đó hay không. 

Một chi tiết quan trọng là cùng một ô có thể được sử dụng lại bất kỳ số lần nào, do đó cấu trúc không phải là vấn đề đường dẫn đơn giản với các ràng buộc đã truy cập. Thay vào đó, đây là vấn đề liệu lưới có chứa bước đi được gắn nhãn phù hợp với trình tự hay không. 

Các ràng buộc định hình mạnh mẽ giải pháp. Lưới có tối đa 100 ô, trong khi mỗi chuỗi truy vấn có độ dài tối đa là 13. Có tới 100000 truy vấn. Sự kết hợp này có nghĩa là chúng tôi có thể cung cấp một chương trình động cho mỗi truy vấn khá nặng trên lưới, miễn là mỗi truy vấn duy trì gần khoảng vài nghìn thao tác. Bất cứ điều gì liên quan đến tìm kiếm theo cấp số nhân trên các đường dẫn mà không ghi nhớ sẽ thất bại ngay lập tức, vì việc phân nhánh theo 8 hướng trong 13 bước sẽ tạo ra tới 8^13 khả năng trong trường hợp xấu nhất. 

Một điểm tinh tế là việc tái sử dụng các ô sẽ loại bỏ cấu trúc “đường dẫn đơn giản” thông thường. Tìm kiếm đầu tiên theo chiều sâu đơn giản không ghi nhớ các trạng thái có thể truy cập lại cùng một cấu hình nhiều lần, đặc biệt là trên các lưới thống nhất nơi tất cả các chữ cái khớp nhau. 

Điểm tinh tế thứ hai là mỗi truy vấn đều độc lập. Chúng tôi không thể mang trạng thái trên các truy vấn vì các chuỗi khác nhau và tính hợp lệ phụ thuộc vào việc căn chỉnh chính xác chuỗi ký tự. 

## Phương pháp tiếp cận 

Giải thích trực tiếp đề xuất bắt đầu từ mọi ô khớp với ký tự đầu tiên của mẫu, sau đó thực hiện tìm kiếm theo chiều sâu trước tiên, thử tất cả 8 bước di chuyển ở mỗi bước trong khi khớp ký tự tiếp theo. Điều này đúng vì nó liệt kê rõ ràng tất cả các bước đi có thể. 

Tuy nhiên, cách tiếp cận này lặp lại các bài toán con tương tự nhiều lần. Từ một ô cố định và một chỉ mục cố định trong chuỗi, câu hỏi còn lại luôn giống nhau: chúng ta có thể tiếp tục khớp hậu tố của chuỗi từ đây không? Nếu không có tính năng ghi nhớ, phép đệ quy sẽ khám phá nhiều đường dẫn dư thừa theo cấp số nhân, đặc biệt là vì việc xem lại các ô được cho phép và các chu kỳ tạo ra khả năng tính toán lại vô hạn. 

Cấu trúc của vấn đề cho phép cải cách rõ ràng hơn. Đối với mỗi chuỗi truy vấn, hãy xác định trạng thái dưới dạng một cặp bao gồm vị trí lưới và chỉ mục trong chuỗi. Từ một trạng thái, quá trình chuyển đổi sẽ chuyển sang bất kỳ ô liền kề nào có chữ cái khớp với ký tự tiếp theo. Vì độ dài chuỗi tối đa là 13 và lưới có nhiều nhất là 100 vị trí nên tổng số trạng thái cho mỗi truy vấn nhiều nhất là 1300. Mỗi trạng thái có tối đa 8 lần chuyển đổi, do đó, việc khám phá đầy đủ bằng tính năng ghi nhớ là tuyến tính trong không gian trạng thái này. 

Quan sát quan trọng là các chu kỳ trong lưới không còn quan trọng một khi chúng ta ghi nhớ các trạng thái. Ngay cả khi một bước đi xem lại một ô, chúng ta không cần tính toán lại xem liệu có thể có hậu tố từ trạng thái đó hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| DFS vũ phu mà không cần ghi nhớ | O(8^L) mỗi truy vấn | O(L) đệ quy | Quá chậm | 
| Trạng thái DP trên (ô, chỉ mục) cho mỗi truy vấn | O(nm · L · 8) mỗi truy vấn | O(nm · L) mỗi truy vấn | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng truy vấn một cách độc lập bằng cách sử dụng lập trình động hoặc công thức DFS được ghi nhớ trên lưới. 

1. Chúng tôi coi mọi ô là điểm khởi đầu tiềm năng cho mẫu. Ô bắt đầu chỉ hợp lệ nếu chữ cái của nó khớp với ký tự đầu tiên của chuỗi. Hạn chế này sẽ loại bỏ ngay lập tức những công việc không cần thiết, vì những lần khởi đầu không khớp không bao giờ có thể dẫn đến một kết quả trùng khớp hợp lệ. 
2. Chúng tôi xác định hàm trên các trạng thái`(r, c, i)`nghĩa là chúng tôi hiện đang ở phòng giam`(r, c)`và chúng tôi đã khớp tiền tố`s[0..i]`, vì vậy ký tự tiếp theo phù hợp là`s[i+1]`. Mục đích là để xác định liệu chúng ta có thể đạt được chỉ số`len(s) - 1`. 
3. Từ một tiểu bang`(r, c, i)`, nếu như`i`đã là chỉ mục cuối cùng của chuỗi, chúng tôi đã khớp thành công toàn bộ mẫu và trả về thành công. 
4. Nếu không, chúng tôi cố gắng di chuyển đến tất cả 8 ô lân cận`(nr, nc)`. Quá trình chuyển đổi chỉ hợp lệ nếu ký tự trong`(nr, nc)`bằng`s[i+1]`. Đối với mỗi hàng xóm hợp lệ, chúng tôi đánh giá đệ quy trạng thái`(nr, nc, i+1)`. 
5. Để tránh tính toán lại cùng một trạng thái nhiều lần, chúng tôi lưu trữ kết quả trong bảng ghi nhớ được lập chỉ mục bởi`(r, c, i)`. Khi một trạng thái được tính toán, nó sẽ được sử dụng lại bất cứ khi nào gặp lại. 
6. Câu trả lời cho truy vấn là “CÓ” nếu bất kỳ vị trí bắt đầu nào dẫn đến việc hoàn thành chuỗi thành công, nếu không thì “KHÔNG”. 

Chi tiết triển khai quan trọng là việc ghi nhớ được thực hiện trên mỗi truy vấn chứ không phải trên toàn bộ. Mỗi chuỗi xác định một không gian trạng thái khác nhau, do đó việc sử dụng lại trên các truy vấn sẽ trộn lẫn các tính toán không liên quan. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ thực tế là mọi bước đi hợp lệ trong lưới đều tương ứng chính xác với một chuỗi các chuyển đổi trong biểu đồ trạng thái được xác định bởi`(cell, index)`. Mọi di chuyển có thể có trong lưới được thể hiện trong bước chuyển tiếp và mọi chuỗi ký tự hợp lệ phải tương ứng với một số đường dẫn trong biểu đồ trạng thái này. 

Việc ghi nhớ không loại bỏ bất kỳ đường dẫn hợp lệ nào vì nó chỉ loại bỏ việc tính toán lại các trạng thái giống hệt nhau chứ không loại bỏ các chuyển đổi. Vì mỗi trạng thái nắm bắt đầy đủ tất cả thông tin cần thiết để tiếp tục nên việc tính toán lại nó không thể thay đổi kết quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

dx = [-1,-1,-1,0,0,1,1,1]
dy = [-1,0,1,-1,1,-1,0,1]

def solve_one(grid, s, n, m):
    L = len(s)
    memo = [[[-1] * L for _ in range(m)] for _ in range(n)]

    def dfs(x, y, i):
        if memo[x][y][i] != -1:
            return memo[x][y][i]

        if i == L - 1:
            memo[x][y][i] = 1
            return 1

        nx_char = s[i + 1]
        for k in range(8):
            nx = x + dx[k]
            ny = y + dy[k]
            if 0 <= nx < n and 0 <= ny < m:
                if grid[nx][ny] == nx_char:
                    if dfs(nx, ny, i + 1):
                        memo[x][y][i] = 1
                        return 1

        memo[x][y][i] = 0
        return 0

    for i in range(n):
        for j in range(m):
            if grid[i][j] == s[0]:
                if dfs(i, j, 0):
                    return True
    return False

def main():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]
    q = int(input())

    for _ in range(q):
        s = input().strip()
        print("YES" if solve_one(grid, s, n, m) else "NO")

if __name__ == "__main__":
    main()
```Việc truyền tải lưới được thực hiện thông qua một mảng bù 8 hướng, giúp cho việc tạo hàng xóm không đổi trong mỗi bước. Bảng ghi nhớ có ba chiều, được khóa theo hàng, cột và vị trí trong chuỗi. Mỗi truy vấn sẽ xây dựng lại bảng này vì các kết quả trước đó không liên quan. 

DFS ngay lập tức lọc các chuyển đổi theo sự bình đẳng về ký tự, đây là yếu tố cắt tỉa chính giúp giải pháp được thực hiện nhanh chóng trong thực tế. Nếu không có sự kiểm tra này, mọi hàng xóm sẽ được khám phá bất kể tính khả thi, nhân công việc lên tới tám ở mỗi bước. 

## Ví dụ đã hoạt động 

Hãy xem xét lưới:```
ab
ju
```và chuỗi truy vấn`auabj`. 

Chúng tôi theo dõi cách DP khám phá các trạng thái có thể bắt đầu từ việc khớp các ô`'a'`. 

| Bước | Vị trí | Chỉ mục | Ký tự tiếp theo | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | (0,0) | 0 | bạn | thử hàng xóm | 
| 2 | (0,1) | 1 | một | di chuyển sang phải | 
| 3 | (1,1) | 2 | b | di chuyển theo đường chéo/hàng xóm hợp lệ | 
| 4 | (0,0) | 3 | j | được phép xem lại | 

Dấu vết này cho thấy việc xem lại các ô được xử lý một cách tự nhiên mà không cần đánh dấu đặc biệt, vì nhận dạng trạng thái bao gồm chỉ mục trong chuỗi. 

Bây giờ hãy xem xét một mẫu thất bại trong đó một ký tự không thể khớp ở bất kỳ bước nào. Giả sử ký tự bắt buộc tiếp theo không tồn tại ở bất kỳ lân cận nào ở bất kỳ trạng thái có thể truy cập nào ở độ sâu nhất định. Việc ghi nhớ đảm bảo rằng một khi tất cả các đường dẫn từ một trạng thái không thành công thì trạng thái đó sẽ không bao giờ được xem xét lại nữa, ngăn cản việc thăm dò ngõ cụt lặp đi lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q · n · m · L · 8) | Mỗi truy vấn khám phá tối đa tất cả`(cell, index)`trạng thái một lần, mỗi trạng thái có tối đa 8 lần chuyển tiếp | 
| Không gian | O(n · m · L) | Bảng ghi nhớ cho mỗi truy vấn lưu trữ kết quả cho tất cả các bộ ba trạng thái | 

Với`n, m ≤ 10`Và`L ≤ 13`, mỗi truy vấn có giá khoảng vài nghìn thao tác, phù hợp thoải mái ngay cả đối với`q = 100000`trong việc triển khai được tối ưu hóa, đặc biệt là trong các ngôn ngữ được biên dịch. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []

    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]
    q = int(input())

    dx = [-1,-1,-1,0,0,1,1,1]
    dy = [-1,0,1,-1,1,-1,0,1]

    sys.setrecursionlimit(10**7)

    def solve_one(grid, s):
        n, m = len(grid), len(grid[0])
        L = len(s)
        memo = [[[-1]*L for _ in range(m)] for _ in range(n)]

        def dfs(x,y,i):
            if memo[x][y][i] != -1:
                return memo[x][y][i]
            if i == L-1:
                memo[x][y][i] = 1
                return 1
            need = s[i+1]
            for k in range(8):
                nx, ny = x+dx[k], y+dy[k]
                if 0 <= nx < n and 0 <= ny < m:
                    if grid[nx][ny] == need:
                        if dfs(nx,ny,i+1):
                            memo[x][y][i] = 1
                            return 1
            memo[x][y][i] = 0
            return 0

        for i in range(n):
            for j in range(m):
                if grid[i][j] == s[0]:
                    if dfs(i,j,0):
                        return True
        return False

    for _ in range(q):
        s = input().strip()
        out.append("YES" if solve_one(grid, s) else "NO")

    return "\n".join(out)

# sample-style sanity checks (placeholders since exact sample formatting omitted)
assert run("""2 2
ab
ju
4
auabj
auau
jbjbjbjb
ajbuabu
""") == "YES\nYES\nYES\nYES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chữ cái đơn dạng lưới 1x1 | CÓ/KHÔNG | hành vi bắt đầu/kết thúc tối thiểu | 
| lặp lại cùng một lưới chữ cái | CÓ cho những lần lặp lại ngắn | xem lại tính chính xác của ô | 
| chữ không thể ở giữa | KHÔNG | cắt tỉa đúng cách | 
| mẫu lặp lại dài | CÓ/KHÔNG tùy theo | xử lý độ sâu lên tới 13 | 

## Vỏ cạnh 

Một lưới chứa đầy ký tự giống nhau nhấn mạnh hành vi tái sử dụng và chu trình. Đối với một mô hình như`"aaaaaaaaaaaaa"`, mọi ô đều là điểm bắt đầu hợp lệ và mọi nước đi vẫn hợp lệ. Thuật toán vẫn kết thúc chính xác vì bảng ghi nhớ đảm bảo rằng mỗi`(cell, index)`cặp chỉ được đánh giá một lần, mặc dù số lần đi đến khái niệm là rất lớn. 

Lưới đơn ô thể hiện việc xử lý ranh giới. Nếu độ dài mẫu lớn hơn 1 thì không thể chuyển đổi vì chuyển động luôn thay đổi ô, vì vậy câu trả lời hợp lệ duy nhất là đối với các chuỗi có độ dài 1 khớp với ký tự đơn. 

Mẫu có ký tự đầu tiên không tồn tại trong lưới sẽ được lọc ngay trước khi DFS bắt đầu. Điều này ngăn chặn việc tạo trạng thái không cần thiết và đảm bảo thuật toán hoàn toàn không tham gia đệ quy trong trường hợp đó.
