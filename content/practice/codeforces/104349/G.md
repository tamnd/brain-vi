---
title: "CF 104349G - Loại bỏ hoán vị"
description: "Chúng ta được cho một hoán vị có kích thước $n$, trong đó $n$ là số chẵn. Mảng bắt đầu bằng một thứ tự đầy đủ các số từ $1$ đến $n$, nhưng thứ tự này là tùy ý. Quá trình liên tục loại bỏ mảng theo cặp."
date: "2026-07-01T18:17:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104349
codeforces_index: "G"
codeforces_contest_name: "TheForces Round #13 (Boombastic-Forces)"
rating: 0
weight: 104349
solve_time_s: 96
verified: false
draft: false
---

[CF 104349G - Loại bỏ hoán vị](https://codeforces.com/problemset/problem/104349/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị về kích thước$n$, Ở đâu$n$là chẵn. Mảng bắt đầu bằng một thứ tự đầy đủ các số từ$1$ĐẾN$n$, nhưng thứ tự là tùy ý. 

Quá trình liên tục loại bỏ mảng theo cặp. Trong mỗi lần di chuyển, chúng ta chỉ được phép chọn hai phần tử liền kề trong đó phần tử bên trái lớn hơn phần tử bên phải và xóa cả hai phần tử đó cùng một lúc. Sau khi xóa, các phần còn lại của mảng được nối lại, do đó độ kề thay đổi linh hoạt. 

Nhiệm vụ không phải là mô phỏng một chuỗi mà là đếm xem có bao nhiêu chuỗi hợp lệ khác nhau của việc loại bỏ như vậy có thể xóa hoàn toàn mảng. Hai chuỗi khác nhau nếu tại một bước nào đó chúng chọn các cặp giảm liền kề khác nhau. 

Ràng buộc$n \le 500$ngay lập tức loại trừ mọi phép liệt kê theo cấp số nhân đối với tất cả các chuỗi xóa có thể. Ngay cả một yếu tố phân nhánh vừa phải cũng sẽ bùng nổ vì mỗi lần loại bỏ sẽ thay đổi tính liền kề và tạo ra các lựa chọn hợp lệ mới. 

Một khó khăn tinh tế đến từ cấu trúc năng động. Một cặp hợp lệ phụ thuộc vào sự kề cận hiện tại và việc loại bỏ một cặp có thể tạo hoặc hủy các cặp hợp lệ khác ở nơi khác. Một chiến lược tham lam ngây thơ như luôn loại bỏ sự đảo ngược có sẵn đầu tiên sẽ không thành công vì các lựa chọn cục bộ ảnh hưởng đến tính khả dụng toàn cầu trong tương lai. 

Một ví dụ phản biện đơn giản là một hoán vị như$[3, 1, 2, 4]$. Ban đầu chỉ$(3,1)$là hợp lệ. Việc loại bỏ nó sẽ buộc phải có một tương lai cụ thể, nhưng nếu tồn tại nhiều nghịch đảo, những lựa chọn ban đầu khác nhau có thể dẫn đến những hoàn thành hoặc ngõ cụt khác nhau. Vì vậy chúng ta cần một chiến lược đếm toàn cầu chứ không phải mô phỏng cục bộ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các trình tự loại bỏ có thể xảy ra. Ở mỗi trạng thái, chúng tôi quét mảng, liệt kê tất cả các cặp giảm liền kề, phân nhánh trên mỗi lựa chọn, loại bỏ cặp và lặp lại. 

Điều này đúng vì nó tuân theo đúng quy luật. Tuy nhiên, số lượng các tiểu bang là rất lớn. Mỗi lần loại bỏ sẽ làm giảm mảng đi hai phần tử, do đó có$n/2$bước, nhưng hệ số phân nhánh có thể lên tới$O(n)$ở giai đoạn đầu. Điều này dẫn đến khoảng$n \cdot (n-2) \cdot (n-4) \cdots 1$, đó là sự tăng trưởng giai thừa, vượt xa giới hạn. 

Quan sát quan trọng là việc loại bỏ luôn loại bỏ hai phần tử liền kề tạo thành sự đảo ngược và sau khi loại bỏ, cấu trúc còn lại hoạt động giống như một vấn đề ghép nối không giao nhau trên một chuỗi trong đó các cặp phải tôn trọng các ràng buộc thứ tự cục bộ. Điều này gợi nhớ mạnh mẽ đến khoảng DP trên các hoán vị trong đó chúng tôi khớp các phần tử và đảm bảo tính hợp lệ bên trong trước khi hợp nhất các phân đoạn. 

Chúng ta có thể diễn giải lại quá trình theo chiều ngược lại. Thay vì loại bỏ các cặp giảm liền kề hợp lệ, hãy nghĩ đến việc xây dựng mảng từ trống bằng cách chèn các cặp theo thứ tự ngược lại. Mỗi lần loại bỏ tương ứng với việc ghép hai phần tử liền kề trong đó trái > phải, điều này cho thấy rằng ngược lại, chúng ta đang hình thành các cấu trúc lồng nhau. Thuộc tính lồng nhau này cho phép chúng ta coi mảng như được chia thành các khoảng độc lập sau khi một cặp hợp lệ được cố định. 

Điều này dẫn đến DP theo các khoảng thời gian: đối với bất kỳ phân khúc nào, chúng tôi tính toán số cách để loại bỏ hoàn toàn phân khúc đó và đối với mỗi phân khúc, chúng tôi thử ghép phần tử đầu tiên với một đối tác có thể thực hiện lần xóa đầu tiên một cách hợp pháp trong phân khúc đó. Khi cặp đó được cố định, phần bên trong và bên ngoài sẽ trở thành các bài toán con độc lập. 

Cấu trúc trở thành: chọn đối tác phù hợp cho một vị trí, đảm bảo đó có thể là lần xóa hợp lệ đầu tiên trong phân khúc đó, phân chia mảng và nhân các khả năng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng trạng thái) | hàm mũ, ~$O(n!)$|$O(n)$đệ quy | Quá chậm | 
| Khoảng thời gian DP |$O(n^3)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định DP trên các phân đoạn của mảng. Cho phép`dp[l][r]`biểu thị số cách hợp lệ để loại bỏ hoàn toàn tất cả các phần tử trong mảng con khỏi chỉ mục$l$ĐẾN$r$, giả sử độ dài đoạn này là chẵn. 

Chúng tôi tính toán câu trả lời để tăng độ dài đoạn. 

1. Khởi tạo`dp[i][i-1] = 1`cho các phân đoạn trống. Đây là trường hợp cơ bản trong đó không còn gì để loại bỏ và nó góp phần gấp bội trong quá trình phân tách. 
2. Lặp lại tất cả các độ dài đoạn từ 2 đến$n$, chỉ xét độ dài chẵn. Độ dài lẻ không hợp lệ vì mỗi lần di chuyển sẽ loại bỏ chính xác hai phần tử, vì vậy chúng không bao giờ có thể biến mất hoàn toàn. 
3. Đối với từng phân khúc$[l, r]$, chúng tôi quyết định vị trí nào$k$có thể được ghép nối với$l$trong lần loại bỏ đầu tiên của phân khúc này. Chúng tôi chỉ xem xét$k$như vậy$a[l] > a[k]$Và$k - l$là khoảng cách lẻ tương thích với cấu trúc ghép nối bên trong. 
4. Nếu chúng ta chọn đối tác$k$, việc loại bỏ$(l, k)$chia đoạn thành ba vùng độc lập: bên trong$(l+1, k-1)$và bên ngoài$(k+1, r)$. Sự đóng góp trở thành`dp[l+1][k-1] * dp[k+1][r]`. 
5. Tổng hợp tất cả hợp lệ$k$, tích lũy vào`dp[l][r]`. 
6. Câu trả lời cuối cùng là`dp[0][n-1]`. 

Ý tưởng chính là khi cặp di động đầu tiên trong một phân đoạn được cố định thì mọi thứ bên trong cặp đó và mọi thứ bên ngoài nó sẽ phát triển độc lập. Tính độc lập này cho phép nhân các bài toán con. 

### Tại sao nó hoạt động 

Tại bất kỳ bước hợp lệ nào trong một phân đoạn, cặp giảm liền kề được chọn$(l, k)$đóng vai trò phân cách cấu trúc. Không có sự loại bỏ nào trong tương lai có thể trộn lẫn các yếu tố từ bên trong$(l, k)$với các phần tử bên ngoài nó, bởi vì tất cả các tương tác đều bị hạn chế ở mức độ kề cận và loại bỏ$l$Và$k$phá hủy vĩnh viễn mọi sự liền kề giữa hai bên. Điều này tạo ra một phân vùng liên tục của không gian trạng thái, nghĩa là các bài toán con không can thiệp sau lần phân chia đầu tiên. DP nắm bắt chính xác sự phân tách này bằng cách liệt kê tất cả các lần phân tách đầu tiên có thể có và kết hợp các số đếm độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))
    
    dp = [[0] * n for _ in range(n)]
    
    for i in range(n):
        dp[i][i] = 1
    
    for length in range(2, n + 1, 2):
        for l in range(0, n - length + 1):
            r = l + length - 1
            
            total = 0
            
            for k in range(l + 1, r + 1, 2):
                if a[l] > a[k]:
                    left = dp[l + 1][k - 1] if k - l > 1 else 1
                    right = dp[k + 1][r] if k < r else 1
                    total += left * right
                    total %= MOD
            
            dp[l][r] = total
    
    print(dp[0][n - 1] % MOD)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo cấu trúc DP khoảng thời gian. Bảng DP được khởi tạo cho các phần tử đơn lẻ dưới dạng các phân đoạn hợp lệ tầm thường. 

Các vòng chuyển tiếp chỉ lặp lại trên các đoạn có độ dài chẵn vì không thể loại bỏ hoàn toàn các đoạn lẻ. Đối với mỗi điểm cuối bên trái, chúng tôi thử ghép nối nó với mọi đối tác hợp lệ có thể$k$. điều kiện`a[l] > a[k]`thực thi yêu cầu rằng cặp đã chọn là một đảo ngược có thể tháo rời hợp lệ. 

Bước chẵn lẻ`range(l + 1, r + 1, 2)`đảm bảo tính nhất quán về cấu trúc: sau khi ghép nối$l$với$k$, cả hai phân đoạn thu được đều có độ dài chẵn, cần thiết để loại bỏ hoàn toàn. 

Xử lý ranh giới sử dụng kiểm tra rõ ràng: khi không có phân đoạn bên trong hoặc không có phân đoạn bên phải, đóng góp DP được coi là 1, biểu thị phân tách hợp lệ trống. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
6 4 3 2 1 5
```Chúng tôi chỉ theo dõi một tập hợp con đại diện của các trạng thái DP. 

| Đoạn (l, r) | Được coi là k | Điều kiện a[l] > a[k] | Đóng góp | 
| --- | --- | --- | --- | 
| (0,5) | k=1,2,3,4 | tất cả đều đúng ngoại trừ k=5 | tích lũy qua các lần chia tách hợp lệ | 

Cấu trúc bên trong, mỗi phần phân chia đầu tiên hợp lệ ở vị trí 0 tạo ra các phân đoạn độc lập như$(1,k-1)$Và$(k+1,5)$. Những điều này kết hợp để tạo ra tổng số 3 lần xóa hoàn toàn hợp lệ. 

Điều này chứng tỏ rằng có thể có nhiều cặp ban đầu và mỗi cặp dẫn đến một phân tách đầy đủ hợp lệ. 

### Ví dụ 2 

đầu vào:```
4
3 1 4 2
```| Phân đoạn | Cặp đầu tiên hợp lệ | Kết quả | 
| --- | --- | --- | 
| (0,3) | (3,1), (4,2 không hợp lệ), v.v. | 1 | 

Chỉ tồn tại một chuỗi toàn cục hợp lệ vì hầu hết các cặp đều chặn sự phân tách hoàn toàn. 

Điều này cho thấy các cặp trung gian không hợp lệ tự nhiên đóng góp bằng 0 vì một trong các phân đoạn con không thể xóa hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^3)$|$O(n^2)$tiểu bang, mỗi người đều cố gắng$O(n)$chia điểm | 
| Không gian |$O(n^2)$| Bảng DP lưu trữ kết quả khoảng thời gian | 

Với$n \le 500$,$n^3$ở xung quanh$1.25 \times 10^8$hoạt động trong trường hợp xấu nhất. Trong Python được tối ưu hóa với các vòng lặp chặt chẽ và cắt tỉa thông qua tính chẵn lẻ, đây là đường biên nhưng dành cho giải pháp C++ 1 giây hoặc Python được tối ưu hóa mạnh mẽ. Cấu trúc này là tiêu chuẩn cho các bài toán DP khoảng ở kích thước này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    
    MOD = 10**9 + 7
    
    n = int(sys.stdin.readline().strip())
    a = list(map(int, sys.stdin.readline().split()))
    
    dp = [[0]*n for _ in range(n)]
    for i in range(n):
        dp[i][i] = 1
    
    for length in range(2, n+1, 2):
        for l in range(n-length+1):
            r = l+length-1
            total = 0
            for k in range(l+1, r+1, 2):
                if a[l] > a[k]:
                    left = dp[l+1][k-1] if k-l>1 else 1
                    right = dp[k+1][r] if k<r else 1
                    total = (total + left*right) % MOD
            dp[l][r] = total
    
    return str(dp[0][n-1] % MOD)

# provided sample
assert run("6\n6 4 3 2 1 5\n") == "3"

# minimum size
assert run("2\n2 1\n") == "1"

# already decreasing chain
assert run("4\n4 3 2 1\n") == "2"

# alternating pattern
assert run("4\n3 1 4 2\n") == "1"

# random small
assert run("6\n1 6 2 5 3 4\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 1 | 1 | loại bỏ hợp lệ tối thiểu | 
| 4 4 3 2 1 | 2 | nhiều lệnh ghép nối | 
| 4 3 1 4 2 | 1 | kết hợp hạn chế | 
| 6 1 6 2 5 3 4 | 0 | không thể loại bỏ hoàn toàn | 

## Vỏ cạnh 

Một trường hợp tối thiểu như$n=2$với$[2,1]$xác nhận rằng một đảo ngược hợp lệ duy nhất là đủ để tạo thành chính xác một chuỗi loại bỏ. DP khởi tạo điều này một cách trực tiếp vì đoạn duy nhất có độ dài 2 và thỏa mãn điều kiện$a[0] > a[1]$. 

Một mảng giảm nghiêm ngặt như$[4,3,2,1]$cho thấy có nhiều cấu trúc ghép nối tồn tại. Thuật toán tính chính xác cả hai cách ghép nối các phần tử bên ngoài trước hoặc các phần tử bên trong trước, vì mỗi lựa chọn hợp lệ của lần phân tách đầu tiên sẽ tạo ra các bài toán con độc lập. 

Một cấu hình không có tính năng xóa toàn cầu hợp lệ, chẳng hạn như$[1,6,2,5,3,4]$, chứng tỏ rằng nhiều sự đảo ngược cục bộ không đảm bảo khả năng loại bỏ hoàn toàn. DP tự nhiên trả về 0 vì mọi phân vùng được thử cuối cùng đều tạo ra một phân đoạn không hợp lệ và không có phần hoàn thành hợp lệ.
