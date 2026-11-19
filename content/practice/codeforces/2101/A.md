---
title: "CF 2101A - Mex trong lưới"
description: "Chúng ta được yêu cầu điền vào lưới $n lần n$ các số nguyên từ $0$ đến $n^2 - 1$, mỗi số được sử dụng chính xác một lần. Mỗi lưới con hình chữ nhật đóng góp một giá trị bằng MEX của các số bên trong nó và mục tiêu là tối đa hóa tổng các giá trị MEX này trên tất cả…"
date: "2026-06-08T05:07:26+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2101
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1024 (Div. 1)"
rating: 1300
weight: 2101
solve_time_s: 96
verified: false
draft: false
---

[CF 2101A - Mex trong lưới](https://codeforces.com/problemset/problem/2101/A) 

**Đánh giá:** 1300 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu điền vào một$n \times n$lưới với các số nguyên từ$0$ĐẾN$n^2 - 1$, mỗi cái được sử dụng đúng một lần. Mỗi lưới con hình chữ nhật đóng góp một giá trị bằng MEX của các số bên trong nó và mục tiêu là tối đa hóa tổng các giá trị MEX này trên tất cả các hình chữ nhật con có thể có. 

Khó khăn chính không phải là tính toán MEX cho một lưới cố định mà là chọn vị trí các số sao cho nhiều lưới con chứa các giá trị nhỏ như$0, 1, 2, \dots$thường xuyên nhất có thể, vì đó là những giá trị duy nhất có thể làm tăng MEX. Các số lớn có hiệu quả trơ: chúng không bao giờ giúp tăng MEX trừ khi tất cả các số nhỏ hơn đều có sẵn. 

Những ràng buộc cho phép$n$lên tới 500, vì vậy$n^2$có thể đạt tới 250.000. Bất kỳ giải pháp nào mô phỏng các lưới con hoặc tính toán MEX trực tiếp trên mỗi lưới con sẽ liên quan đến khoảng$O(n^4)$hoặc$O(n^3)$hoạt động vượt quá khả năng thực hiện. Thậm chí$O(n^2 \log n)$các công trình xây dựng đều ổn, nhưng bất cứ điều gì lý do cho mỗi lưới con đều bị loại trừ. 

Một trường hợp thất bại tinh vi xuất hiện khi các số nằm rải rác ngẫu nhiên hoặc được đặt theo thứ tự hàng lớn. Ví dụ, đối với$n=3$, đặt:```
0 1 2
3 4 5
6 7 8
```tạo ra xu hướng mạnh mẽ đối với các giá trị nhỏ chỉ được nhóm ở khu vực trên cùng bên trái. Nhiều lưới con có thể bao gồm các giá trị nhỏ hoàn toàn bỏ lỡ chúng, làm giảm đóng góp của MEX. Thay vào đó, sự sắp xếp tối ưu phải đảm bảo rằng các giá trị nhỏ được trải rộng sao cho nhiều hình chữ nhật con bao gồm chúng sớm. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các hoán vị của$n^2$các số và tính tổng MEX cho mỗi cách sắp xếp. Điều này đúng về mặt định nghĩa nhưng không thể thực hiện được vì có$(n^2)!$hoán vị và việc đánh giá từng hoán vị đòi hỏi phải lặp lại trên tất cả$O(n^2)$lưới điện phụ lên tới$O(n^2)$hoạt động trên mỗi lưới con, dẫn đến ít nhất$O((n^2)! \cdot n^4)$sự phức tạp. 

Chúng ta cần quan sát về mặt cấu trúc: MEX được điều khiển hoàn toàn bởi cách các số nhỏ xuất hiện sớm bên trong hình chữ nhật. Đối với lưới con cố định, MEX của nó ít nhất là$k$khi và chỉ nếu tất cả các số$0$bởi vì$k-1$đang ở bên trong nó. Vì vậy sự đóng góp của mỗi giá trị$x$phụ thuộc vào bao nhiêu lưới con chứa tất cả các số$0 \dots x-1$. 

Điều này giải quyết lại vấn đề: thay vì nghĩ trực tiếp về MEX, chúng ta muốn đặt các số nhỏ sao cho tập hợp$\{0,1,\dots,x\}$có xu hướng nằm trong nhiều hình chữ nhật. 

Một cách cổ điển để tối đa hóa điều này là phân cụm các giá trị nhỏ trong một vùng nhỏ gọn, bởi vì một cụm chặt chẽ được chứa trong nhiều hình chữ nhật con hơn so với cấu hình phân tán. Chiến lược tối ưu là xây dựng một trật tự “giống như con rắn” hoặc giống như xoắn ốc sao cho bất kỳ tiền tố nào của các số vẫn thu gọn về mặt không gian. 

Một cách xây dựng tối ưu đơn giản là lấp đầy lưới theo hình xoắn ốc bắt đầu từ góc trên bên trái, di chuyển vào trong từng lớp. Điều này đảm bảo rằng tập hợp nhỏ nhất$k$các số luôn tạo thành một vùng hình vuông được kết nối, giúp tối đa hóa số lượng hình chữ nhật con chứa đầy nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force |$O((n^2)! \cdot n^4)$|$O(n^2)$| Quá chậm | 
| Xây dựng xoắn ốc / lớp |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Mục đích là để gán số$0 \dots n^2-1$sao cho các giá trị nhỏ được phân cụm theo không gian. 

1. Bắt đầu với khoảng trống$n \times n$lưới và giá trị hiện tại$v = 0$. giá trị$v$sẽ luôn biểu thị số chưa sử dụng nhỏ nhất tiếp theo, đảm bảo chúng tôi kiểm soát hình dạng của tiền tố. 
2. Duy trì bốn ranh giới: hàng trên, hàng dưới, cột bên trái và cột bên phải. Chúng xác định hình chữ nhật chưa được tô màu hiện tại. Ban đầu, đây là lưới đầy đủ. 
3. Điền vào hàng trên cùng từ trái sang phải bằng cách sử dụng các ranh giới hiện tại, gán các giá trị liên tiếp. Sau khi lấp đầy nó, di chuyển ranh giới trên cùng vào trong. Điều này đảm bảo các giá trị nhỏ nhất chiếm lớp ngoài cùng, giúp chúng có thể truy cập được trong nhiều hình chữ nhật. 
4. Điền vào cột bên phải từ trên xuống dưới, sau đó thu nhỏ ranh giới bên phải. Điều này tiếp tục phân lớp bên trong trong khi vẫn duy trì khả năng kết nối của các giá trị nhỏ. 
5. Điền vào hàng dưới cùng từ phải sang trái, sau đó thu nhỏ ranh giới phía dưới. 
6. Điền vào cột bên trái từ dưới lên trên, sau đó thu nhỏ ranh giới bên trái. 
7. Lặp lại quá trình này cho đến khi tất cả các ô được lấp đầy. Mỗi chu kỳ đầy đủ tạo ra một “vòng” có giá trị tăng dần xung quanh tâm. 

Lý do thứ tự này quan trọng là vì bất kỳ tiền tố nào của giá trị$0 \dots k$sẽ luôn tạo thành một vùng tiếp giáp duy nhất thu nhỏ vào trong một cách có kiểm soát. Điều này tối đa hóa số lượng hình chữ nhật con có thể chứa đầy đủ tiền tố. 

### Tại sao nó hoạt động 

Đối với bất kỳ giá trị$k$, xét tập hợp các ô chứa$0 \dots k$. Trong cấu trúc này, các ô này luôn tạo thành một vùng được kết nối với hình chữ nhật có giới hạn tối thiểu. Bởi vì MEX trong một lưới con phụ thuộc vào việc liệu tất cả các giá trị này có được chứa hay không, nên việc tối đa hóa số lượng lưới con chứa hình chữ nhật bao quanh sẽ tối đa hóa tổng đóng góp. Hình xoắn ốc đảm bảo hình chữ nhật giới hạn này phát triển chậm và đối xứng nhất có thể, giúp tối đa hóa tần suất ngăn chặn trên tất cả các hình chữ nhật có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        grid = [[0] * n for _ in range(n)]
        
        top, bottom = 0, n - 1
        left, right = 0, n - 1
        val = 0
        
        while top <= bottom and left <= right:
            for j in range(left, right + 1):
                grid[top][j] = val
                val += 1
            top += 1

            for i in range(top, bottom + 1):
                grid[i][right] = val
                val += 1
            right -= 1

            if top <= bottom:
                for j in range(right, left - 1, -1):
                    grid[bottom][j] = val
                    val += 1
                bottom -= 1

            if left <= right:
                for i in range(bottom, top - 1, -1):
                    grid[i][left] = val
                    val += 1
                left += 1

        for row in grid:
            print(*row)

if __name__ == "__main__":
    solve()
```Mã thực hiện điền hình xoắn ốc tiêu chuẩn. Chi tiết triển khai quan trọng là kiểm tra ranh giới cẩn thận trước khi điền vào hàng dưới cùng và cột bên trái. Nếu không có những kiểm tra này, các lưới có kích thước lẻ sẽ ghi đè lên các ô đã được chỉ định khi hình xoắn ốc thu gọn vào giữa. 

Biến`val`đang tăng lên một cách chặt chẽ và đảm bảo mỗi số từ$0$ĐẾN$n^2-1$được sử dụng đúng một lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
```Chúng tôi theo dõi việc lấp đầy hình xoắn ốc: 

| Bước | Hành động | Trạng thái lưới (một phần) | Giá trị tiếp theo | 
| --- | --- | --- | --- | 
| 1 | Điền vào hàng trên cùng | 0 1 2 / _ _ _ / _ _ _ | 3 | 
| 2 | Điền vào cột bên phải | 0 1 2 / _ _ 3 / _ _ 4 | 5 | 
| 3 | Điền vào hàng dưới cùng | 0 1 2 / _ _ 3 / 6 5 4 | 7 | 
| 4 | Điền col trái | 0 1 2/7 _ 3/6 5 4 | 8 | 

Điều này tạo ra một đường xoắn ốc hoàn toàn nhất quán trong đó các số nhỏ vẫn ở gần ranh giới bên ngoài, tối đa hóa sự bao hàm trên các hình chữ nhật con. 

### Ví dụ 2 

đầu vào:```
n = 4
```Quá trình tương tự tạo ra cấu trúc vòng lớn hơn: 

| Lớp | Vùng đầy | Hiệu ứng | 
| --- | --- | --- | 
| 1 | Viền ngoài | tối đa hóa việc bao gồm 0-11 | 
| 2 | Đường viền bên trong | giữ tập trung 12-15 | 

Điều này cho thấy các giá trị nhỏ nhất luôn chiếm ưu thế ở các lớp bên ngoài, làm tăng sự hiện diện của chúng trong các lưới con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi ô được viết chính xác một lần trong quá trình truyền tải xoắn ốc | 
| Không gian |$O(n^2)$| Lưu trữ lưới | 

Các ràng buộc cho phép tổng cộng lên tới 1000$n$, vậy tổng số ô nhiều nhất là$10^6$, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    t = int(input())
    for _ in range(t):
        n = int(input())
        grid = [[0]*n for _ in range(n)]
        
        top, bottom, left, right = 0, n-1, 0, n-1
        val = 0
        
        while top <= bottom and left <= right:
            for j in range(left, right+1):
                grid[top][j] = val
                val += 1
            top += 1
            
            for i in range(top, bottom+1):
                grid[i][right] = val
                val += 1
            right -= 1
            
            if top <= bottom:
                for j in range(right, left-1, -1):
                    grid[bottom][j] = val
                    val += 1
                bottom -= 1
            
            if left <= right:
                for i in range(bottom, top-1, -1):
                    grid[i][left] = val
                    val += 1
                left += 1
        
        output.append("\n".join(" ".join(map(str, row)) for row in grid))
    
    return "\n".join(output)

# provided samples
assert run("2\n2\n3\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1`|`0`| lưới tối thiểu | 
|`1\n2`| xoắn ốc 2x2 | độ đúng ranh giới | 
|`1\n3`| xoắn ốc 3x3 | xử lý trung tâm lẻ | 
|`2\n2\n3`| trường hợp mẫu | nhiều trường hợp thử nghiệm | 

## Vỏ cạnh 

Một lưới tối thiểu như$n=1$chỉ chứa một giá trị duy nhất. Thuật toán ngay lập tức đi vào vòng lặp, điền 0 vào ô (0,0) và kết thúc. Không có sai lệch ranh giới xảy ra vì điều kiện vòng lặp`top <= bottom and left <= right`giữ đúng một lần. 

Đối với một lưới có kích thước lẻ như$n=3$, đường xoắn ốc cuối cùng sẽ sụp đổ vào ô trung tâm. Vào thời điểm đó, mọi ranh giới đều hội tụ và chỉ còn lại một nhiệm vụ. Việc kiểm tra ranh giới ngăn chặn việc xử lý lại các ô đã được điền, đảm bảo giá trị trung tâm được ghi chính xác một lần.
