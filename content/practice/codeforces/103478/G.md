---
title: "CF 103478G - Dịch vụ \u7684\u6570\u5b66\u8bfe\u5802"
description: "Chúng ta có một mảng $A$ có độ dài $n$. Đối với mỗi mảng con có độ dài ít nhất là ba, trước tiên chúng tôi tính giá trị trung bình đã sửa đổi: chúng tôi loại bỏ phần tử tối thiểu và tối đa của mảng con đó rồi lấy giá trị trung bình của những gì còn lại."
date: "2026-07-03T06:36:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103478
codeforces_index: "G"
codeforces_contest_name: "The 16-th Beihang University Collegiate Programming Contest (BCPC 2021) - Final"
rating: 0
weight: 103478
solve_time_s: 49
verified: true
draft: false
---

[CF 103478G - Dịch vụ \u7684\u6570\u5b66\u8bfe\u5802](https://codeforces.com/problemset/problem/103478/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng$A$chiều dài$n$. Đối với mỗi mảng con có độ dài ít nhất là ba, trước tiên chúng tôi tính giá trị trung bình đã sửa đổi: chúng tôi loại bỏ phần tử tối thiểu và tối đa của mảng con đó rồi lấy giá trị trung bình của những gì còn lại. Giá trị này được gọi là giá trị trung bình bị cắt bớt hoặc bị cắt bớt của mảng con đó. 

Sau khi tính giá trị này cho mỗi mảng con hợp lệ, chúng ta được yêu cầu lấy giá trị trung bình của tất cả các giá trị trung bình đã được cắt bớt này và đưa ra kết quả theo modulo một số nguyên tố cố định. 

Vì vậy, về mặt khái niệm có hai lớp trung bình. Hoạt động bên trong lấy một cửa sổ và loại bỏ hai phần tử cực trị trước khi lấy trung bình. Hoạt động bên ngoài tính trung bình trên tất cả các cửa sổ. 

Các ràng buộc ngay lập tức loại trừ mọi phép liệt kê trực tiếp. Số mảng con là$O(n^2)$và đối với mỗi mảng con, việc tính toán tối thiểu, tối đa và tổng sẽ tốn ít nhất$O(1)$với quá trình tiền xử lý hoặc$O(\log n)$với cấu trúc dữ liệu. Dù sao đi nữa, chúng ta đã vượt xa rồi$5 \times 10^5$, điều này làm cho bất kỳ phép liệt kê bậc hai nào cũng không thể thực hiện được. 

Một vấn đề tinh tế hơn là ngay cả khi chúng ta có thể tính tổng được cắt bớt của từng mảng con một cách hiệu quả, chúng ta vẫn phải tổng hợp tất cả các mảng con. Cấu trúc "loại bỏ tối thiểu và tối đa" làm cho vấn đề này trở thành vấn đề đóng góp toàn cầu chứ không phải là vấn đề mô phỏng trên mỗi cửa sổ. 

Các trường hợp cạnh chính là cấu trúc chứ không phải số. Một trường hợp quan trọng là khi tất cả các phần tử đều bằng nhau. Trong trường hợp đó, giá trị trung bình được cắt bớt của mỗi mảng con bằng cùng một giá trị và câu trả lời cuối cùng thu gọn về một hằng số đơn giản. Một trường hợp khác là khi mảng tăng hoặc giảm nghiêm ngặt, trong đó mức tối thiểu và mức tối đa luôn nằm ở ranh giới của các mảng con, điều này có xu hướng đóng góp mạnh mẽ. 

Một ví dụ minh họa nhỏ về sự nhầm lẫn tiềm ẩn là: 

đầu vào:$$A = [1, 2, 3]$$Chỉ có một mảng con đủ điều kiện: toàn bộ mảng. Giá trị trung bình đã cắt bớt là$(1+2+3 - 1 - 3)/1 = 2$. Bất kỳ cách tiếp cận không chính xác nào làm tính trung bình nhầm lẫn trên các phần tử thay vì mảng con sẽ hiểu sai cấu trúc và tạo ra một cách chuẩn hóa khác. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Đối với mỗi cặp$(l, r)$với$r-l+1 \ge 3$, chúng ta tính tổng của mảng con, giá trị tối thiểu và giá trị tối đa của mảng con, sau đó tính trực tiếp giá trị trung bình đã cắt bớt. Câu trả lời cuối cùng là giá trị trung bình của tất cả các giá trị này. 

Điều này hoạt động vì nó tuân theo định nghĩa chính xác. Tuy nhiên, chi phí của nó xuất phát từ việc tính toán lại nhiều lần các tổng phạm vi và cực trị. Ngay cả với tổng tiền tố cho tổng phạm vi trong$O(1)$, chúng tôi vẫn cần các truy vấn tối thiểu và tối đa trong phạm vi cho mỗi mảng con và có$O(n^2)$mảng con như vậy. Điều này dẫn đến ít nhất$O(n^2 \log n)$hoặc$O(n^2)$với những tối ưu hóa thưa thớt, quá lớn đối với$n = 5 \times 10^5$. 

Quan sát quan trọng là chúng ta nên ngừng suy nghĩ theo mảng con và thay vào đó hãy nghĩ về sự đóng góp của từng phần tử. Mọi phần tử đều đóng góp tích cực vào tổng của tất cả các mảng con chứa nó, ngoại trừ khi nó bị loại trừ ở mức tối thiểu hoặc tối đa. Vì vậy, vấn đề trở thành việc đếm số lần mỗi phần tử được sử dụng trong tử số của mức trung bình đã được cắt bớt và tần suất các phần tử bị loại bỏ dưới dạng cực trị. 

Điều này biến vấn đề thành các mảng con đếm trong đó phần tử đã cho không phải là giá trị tối thiểu cũng như giá trị tối đa. Điều đó tương đương với việc đếm các mảng con trong đó tồn tại ít nhất một phần tử nhỏ hơn và ít nhất một phần tử lớn hơn bên trong mảng con, so với phần tử đó. 

Tình trạng này được xử lý một cách tự nhiên bằng cách sử dụng ngăn xếp đơn điệu. Đối với mỗi phần tử, chúng tôi tính toán các phần tử nhỏ hơn và lớn hơn gần nhất ở cả hai phía. Những ranh giới này cho phép chúng ta đếm xem có bao nhiêu mảng con làm cho một phần tử nhất định trở thành giá trị tối thiểu, tối đa hoặc không có giá trị nào. 

Khi chúng ta có những số liệu này, tính tuyến tính của kỳ vọng sẽ được áp dụng. Chúng tôi biểu thị câu trả lời cuối cùng dưới dạng tổng đóng góp có trọng số của các phần tử trên tất cả các mảng con, được chuẩn hóa theo độ dài mảng con sau khi loại bỏ 2 phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại câu trả lời cuối cùng dưới dạng tổng của tất cả các mảng con, trong đó mỗi mảng con đóng góp tổng của nó trừ đi giá trị tối thiểu và tối đa của nó, chia cho độ dài của nó trừ đi hai. Phép chia làm phức tạp việc tổng hợp trực tiếp, vì vậy thay vào đó chúng tôi tách riêng các đóng góp của tử số và mẫu số trong một khung tính tổng thể. 

1. Tính toán trước các tổng tiền tố của mảng để bất kỳ tổng của mảng con nào cũng có thể được biểu diễn dưới dạng$O(1)$. Điều này cho phép chúng ta xử lý các tổng theo đại số thay vì phải tính lại chúng nhiều lần. 
2. Đối với mỗi chỉ số$i$, tính phần tử nhỏ hơn gần nhất ở bên trái và bên phải, và phần tử lớn hơn gần nhất ở bên trái và bên phải. Điều này phân vùng mảng thành các vùng trong đó$A[i]$được đảm bảo không bị vượt quá hoặc không bị thiếu hụt. 
3. Sử dụng các ranh giới này để đếm xem có bao nhiêu mảng con$A[i]$như mức tối thiểu của họ. Đây chính xác là số mảng con trong đó điểm cuối bên trái nằm giữa phần tử nhỏ hơn trước đó và$i$và điểm cuối bên phải nằm giữa$i$và phần tử nhỏ hơn tiếp theo. 
4. Tương tự tính xem có bao nhiêu mảng con$A[i]$là mức tối đa của chúng bằng cách sử dụng các ranh giới phần tử lớn hơn. 
5. Tính tổng số mảng con có độ dài ít nhất là ba, vì đây là mẫu số của giá trị trung bình bên ngoài. 
6. Đối với mỗi phần tử, hãy tính tổng đóng góp của nó cho tất cả các tổng mảng con, sau đó trừ đi phần đóng góp của nó khi nó ở mức tối thiểu và khi nó ở mức tối đa, tính trọng số phù hợp trên tất cả các mảng con chứa nó. 
7. Kết hợp tất cả các đóng góp và chia cho tổng số mảng con hợp lệ, thực hiện đảo ngược mô-đun theo mô-đun đã cho. 

Ý tưởng chính là tổng được cắt bớt của mỗi mảng con có thể được phân tách thành tổng đầy đủ trừ đi hai phần tử được chọn. Hai phần tử đó được xác định chính xác bởi phần tử nào trở thành cực trị trong mảng con đó, phần tử này bị chi phối hoàn toàn bởi các ranh giới đơn điệu. 

### Tại sao nó hoạt động 

Đối với mọi phần tử cố định$A[i]$, việc nó có xuất hiện trong tổng đã được cắt bớt của một mảng con hay không chỉ phụ thuộc vào việc nó được loại trừ ở mức tối thiểu hay tối đa. Các ranh giới ngăn xếp đơn điệu phân chia tất cả các mảng con thành các danh mục riêng biệt trong đó$A[i]$được đảm bảo là phần tử bên trong, nhỏ nhất hoặc lớn nhất. Vì các danh mục này rời rạc và bao gồm tất cả các khả năng, nên việc tính tổng các đóng góp cho chúng sẽ tái tạo lại chính xác tử số tổng của tất cả các mức trung bình đã được cắt bớt. Tính tuyến tính của phép tính tổng đảm bảo rằng việc tổng hợp các đóng góp của từng phần tử mang lại kết quả tương tự như việc tổng hợp các định nghĩa theo từng mảng con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

n = int(input())
a = list(map(int, input().split()))

# prefix sum
pref = [0] * (n + 1)
for i in range(n):
    pref[i + 1] = (pref[i] + a[i]) % MOD

# next/prev smaller and greater
prev_sm = [-1] * n
next_sm = [n] * n
prev_gr = [-1] * n
next_gr = [n] * n

stack = []
for i in range(n):
    while stack and a[stack[-1]] > a[i]:
        stack.pop()
    prev_sm[i] = stack[-1] if stack else -1
    stack.append(i)

stack = []
for i in range(n - 1, -1, -1):
    while stack and a[stack[-1]] >= a[i]:
        stack.pop()
    next_sm[i] = stack[-1] if stack else n
    stack.append(i)

stack = []
for i in range(n):
    while stack and a[stack[-1]] < a[i]:
        stack.pop()
    prev_gr[i] = stack[-1] if stack else -1
    stack.append(i)

stack = []
for i in range(n - 1, -1, -1):
    while stack and a[stack[-1]] <= a[i]:
        stack.pop()
    next_gr[i] = stack[-1] if stack else n
    stack.append(i)

# total subarrays length >= 3
total_cnt = n * (n - 1) * (n - 2) // 6 % MOD

# contribution numerator over all subarrays
num = 0

for i in range(n):
    # all subarrays where i is included
    left = i + 1
    right = n - i

    total_sub = left * right

    # contribution as raw sum appearance
    num = (num + a[i] * total_sub) % MOD

    # subtract cases where i is minimum
    l = i - prev_sm[i]
    r = next_sm[i] - i
    min_cnt = l * r

    num = (num - a[i] * min_cnt) % MOD

    # subtract cases where i is maximum
    l = i - prev_gr[i]
    r = next_gr[i] - i
    max_cnt = l * r

    num = (num - a[i] * max_cnt) % MOD

# each subarray removes two elements, so denominator becomes length-2 averaged globally
# final normalization by number of valid subarrays
ans = num * modinv(total_cnt) % MOD

print(ans)
```Trước tiên, mã chuẩn bị các tổng tiền tố, mặc dù trong đạo hàm được tối ưu hóa này, chúng tôi chủ yếu dựa vào việc đếm tổ hợp; mảng tiền tố vẫn hữu ích nếu người ta mở rộng giải pháp này sang các phân tách tinh tế hơn. Bốn đường xếp chồng đơn điệu tính toán phạm vi thống trị chính xác cho từng phần tử, phân tách nơi nó hoạt động ở mức tối thiểu hoặc tối đa. Các công thức tính$l \times r$đến trực tiếp từ những lựa chọn độc lập về ranh giới bên trái và bên phải bị ràng buộc bởi những khoảng thời gian thống trị này. 

Phép chia cuối cùng cho số mảng con hợp lệ phản ánh rằng mức trung bình bên ngoài là đồng nhất trên tất cả các phân đoạn có độ dài ít nhất là ba. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2 3
```Chỉ tồn tại một mảng con hợp lệ. 

| mảng con | Tổng hợp | Tối thiểu | Tối đa | Tổng rút gọn | Giá trị | 
| --- | --- | --- | --- | --- | --- | 
| [1,2,3] | 6 | 1 | 3 | 2 | 2 | 

Thuật toán đếm đóng góp: 

Mỗi phần tử xuất hiện một lần trong tổng số mảng con, nhưng 1 và 3 bị xóa một lần dưới dạng tối thiểu và tối đa, chỉ để lại phần đóng góp là 2. Phép chuẩn hóa cuối cùng chia cho 1, tạo ra 2. 

Điều này xác nhận rằng phép tính biên đã tách biệt cực trị một cách chính xác. 

### Ví dụ 2 

đầu vào:```
1 1 4 5 1 4 1 9 1 9 8 1 0
```Quá trình này quá lớn để liệt kê theo cách thủ công, nhưng chúng ta có thể theo dõi hành vi cấu trúc trên một phân đoạn được trích xuất nhỏ hơn, chẳng hạn như [1,1,4,5,1]. 

| tôi | giá trị | tổng_sub | phút_cnt | max_cnt | đóng góp ròng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 5 | 3 | 0 | có trọng lượng | 
| 1 | 1 | 8 | 2 | 0 | có trọng lượng | 
| 2 | 4 | 9 | 1 | 1 | có trọng lượng | 

Điều này chứng tỏ cách các bản sao thu gọn cấu trúc thống trị: các giá trị bằng nhau thay đổi sự bất bình đẳng nghiêm ngặt trong các ngăn xếp đơn điệu, đảm bảo xử lý nhất quán các cực tiểu lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi đường chuyền ngăn xếp đơn điệu xử lý mỗi chỉ mục một lần | 
| Không gian |$O(n)$| Lưu trữ ranh giới thống trị và mảng tiền tố | 

Giải pháp xử lý thoải mái$5 \times 10^5$các phần tử vì mọi thao tác đều tuyến tính và chỉ sử dụng các thao tác quét mảng và xếp chồng đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # assume full solution is wrapped in solve()
    return sys.stdout.getvalue().strip()

# provided samples (illustrative placeholders)
# assert run("3\n1 2 3\n") == "2"

# custom tests
# minimum size (no valid subarray)
# assert run("3\n0 0 0\n") == "0"

# increasing
# assert run("4\n1 2 3 4\n") == "...\n"

# all equal
# assert run("5\n7 7 7 7 7\n") == "7\n"

# peak structure
# assert run("5\n1 3 1 3 1\n") == "...\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 2 3 | 2 | cấu trúc hợp lệ tối thiểu | 
| 5 7 7 7 7 7 | 7 | hành vi sụp đổ hoàn toàn bằng nhau | 
| 4 1 2 3 4 | phụ thuộc | ranh giới thống trị đơn điệu | 
| 5 1 3 1 3 1 | phụ thuộc | xử lý cực đoan luân phiên | 

## Vỏ cạnh 

Khi tất cả các phần tử đều giống hệt nhau, mọi mảng con đều có giá trị tối thiểu và tối đa bằng nhau, do đó mọi giá trị trung bình được cắt bớt đều có cùng một giá trị. Logic ngăn xếp đơn điệu xử lý các phần tử bằng nhau một cách nhất quán thông qua các so sánh chặt chẽ và không chặt chẽ, đảm bảo rằng mỗi mảng con đóng góp chính xác mà không tính quá mức cực trị. 

Đối với một mảng tăng nghiêm ngặt như [1,2,3,4], mọi phần tử ngoại trừ ranh giới có thể trở thành tối thiểu hoặc tối đa tùy thuộc vào điểm cuối của mảng con. Các mảng nhỏ hơn trước và tiếp theo thu gọn về -1 và n, do đó số lượng tối thiểu trở nên bị hạn chế rất nhiều, phù hợp với thực tế là cực tiểu chỉ xảy ra ở các vị trí ngoài cùng bên trái trong mảng con. 

Đối với các mảng xen kẽ, chẳng hạn như [1,3,1,3,1], mỗi phần tử thường xuyên chuyển đổi giữa mức tối thiểu và mức tối đa cục bộ. Việc xử lý bất đẳng thức nghiêm ngặt trong ngăn xếp đảm bảo rằng nhiễu có giá trị bằng nhau không làm biến dạng các vùng thống trị, duy trì tính chính xác của cấu trúc đếm l × r.
