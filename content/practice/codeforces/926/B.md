---
problem: 926B
contest_id: 926
problem_index: B
name: "Add Points"
contest_name: "VK Cup 2018 - Wild-card Round 1"
rating: 1800
tags: ["math", "number theory"]
answer: passed_samples
verified: true
solve_time_s: 68
date: 2026-06-18
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a32df8a-1470-83ec-aaf1-85cacf87cba3
---

#CF 926B - Cộng điểm 

**Đánh giá:** 1800 
**Tags:** toán, lý thuyết số 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a32df8a-1470-83ec-aaf1-85cacf87cba3 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta có một số điểm phân biệt được đặt trên trục số. Vị trí của họ là tùy ý và không được sắp xếp. Nhiệm vụ là chèn số điểm bổ sung tối thiểu sao cho sau khi sắp xếp tất cả các điểm gốc và điểm được chèn, mọi cặp điểm liên tiếp đều có khoảng cách chính xác như nhau. 

Nói cách khác, chúng tôi muốn chuyển đổi một tập hợp tọa độ rải rác thành một cấp số cộng cách đều nhau một cách hoàn hảo bằng cách chỉ thêm các điểm mới, không bao giờ di chuyển các điểm hiện có. Tập cuối cùng phải tạo thành một chuỗi như$a, a + d, a + 2d, \dots$trong đó mọi điểm ban đầu xuất hiện ở đâu đó trong tiến trình này. 

Hạn chế chính là$n \le 100{,}000$, điều này ngay lập tức loại trừ bất kỳ chiến lược bậc hai nào đối với các cặp điểm. Bất cứ điều gì thử tất cả các cặp điểm làm ứng cử viên cho khoảng cách và sau đó mô phỏng cấu trúc đầy đủ đều phải được giảm thiểu một cách cẩn thận, vì việc tái cấu trúc đơn giản có thể yêu cầu$O(n^2)$hoặc tệ hơn trong trường hợp xấu nhất. 

Một khó khăn tinh tế là các điểm ban đầu không được sắp xếp theo thứ tự. Nếu chúng ta quên sắp xếp chúng trước, chúng ta không thể suy luận về khoảng cách “liền kề”. Một dạng lỗi khác xuất phát từ việc giả định rằng khoảng cách tổng thể được xác định bởi sự khác biệt liền kề trong thứ tự đầu vào, điều này là tùy ý. 

Một trường hợp cạnh nhỏ nhưng quan trọng xuất hiện khi các điểm đã cách đều nhau. Ví dụ: đầu vào:```
3
0 2 4
```Ở đây không nên thêm điểm. Bất kỳ cách tiếp cận nào luôn cố gắng “lấp đầy khoảng trống” mà không kiểm tra tính nhất quán của khoảng cách trước tiên sẽ chèn điểm không chính xác. 

Một trường hợp khác:```
3
-5 10 5
```Sắp xếp mang lại$-5, 5, 10$. Bước tự nhiên là 5, nhưng khoảng cách từ 5 đến 10 là 5 trong khi -5 đến 5 cũng cách nhau 10, vì vậy chúng ta phải đưa ra 0. Một chiến lược ngây thơ chỉ nhìn vào mức tối thiểu và tối đa có thể giả định sai bước 7.5 hoặc tương tự và thất bại. 

## Phương pháp tiếp cận 

Quan điểm vũ phu bắt đầu bằng cách chọn hai điểm hiện có làm phần tử liên tiếp tiềm năng của cấp số cộng cuối cùng. Nếu chúng ta giả sử hai cái đó là lân cận trong sự sắp xếp cuối cùng, thì sự khác biệt của chúng sẽ xác định kích thước bước ứng cử viên. Đối với mỗi ứng cử viên như vậy, chúng tôi cố gắng tạo ra cấp số cộng đầy đủ bao gồm khoảng từ tối thiểu đến tối đa và đếm xem có bao nhiêu điểm ban đầu bị thiếu trong khoảng đó. 

Điều này có hiệu quả vì mọi cấu hình cuối cùng hợp lệ đều phải là một cấp số cộng chứa tất cả các điểm ban đầu, do đó bước của nó phải phù hợp với sự khác biệt giữa các điểm liên tiếp trong chuỗi cuối cùng được sắp xếp. Tuy nhiên, thử tất cả các cặp sẽ mang lại$O(n^2)$các ứng cử viên và đối với mỗi ứng cử viên, chúng tôi có thể quét tới$O(n)$điểm để xác nhận phạm vi bảo hiểm, dẫn đến$O(n^3)$theo cách giải thích tồi tệ nhất hoặc ít nhất$O(n^2)$với hàm băm, quá chậm đối với$10^5$. 

Quan sát quan trọng là chúng ta không cần tất cả các cặp. Nếu chúng ta sắp xếp mảng, kích thước bước chính xác phải chia tất cả sự khác biệt giữa các phần tử liền kề trong tiến trình cuối cùng. Điều đó có nghĩa là bước này phải là ước số chung lớn nhất của tất cả các hiệu liền kề trong mảng được sắp xếp. Khi chúng tôi tính toán bước tổng thể này$d$, cấu trúc cuối cùng được xác định hoàn toàn: đó là cấp số cộng từ cực tiểu đến cực đại với bước$d$. 

Sau đó, chúng ta có thể đếm xem có bao nhiêu số nguyên bị thiếu giữa các điểm hiện có bằng cách lặp qua mảng đã sắp xếp và tính tổng số bước bị bỏ qua giữa các phần tử liên tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$hoặc tệ hơn |$O(1)$-$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Chiến lược tối ưu 

1. Sắp xếp tất cả tọa độ theo thứ tự tăng dần. Điều này làm cho khái niệm “điểm lân cận” được xác định rõ ràng trong cấu trúc mục tiêu cuối cùng. Nếu không sắp xếp, sự khác biệt sẽ không có ý nghĩa cấu trúc. 
2. Tính sự khác biệt giữa mỗi cặp liên tiếp trong mảng được sắp xếp. Những khác biệt này thể hiện khoảng cách giữa các điểm trong cấu hình hiện tại và bất kỳ khoảng cách cuối cùng hợp lệ nào cũng phải phân chia tất cả chúng. 
3. Tính ước số chung lớn nhất của tất cả những hiệu này. Giá trị này trở thành kích thước bước thống nhất tối đa có thể mà vẫn cho phép tất cả các điểm ban đầu nằm trên lưới. Bước nhỏ hơn vẫn có tác dụng nhưng sẽ yêu cầu chèn nhiều điểm hơn, vì vậy bước gcd là tối ưu. 
4. Hãy để gcd này là$d$. Tiến trình mục tiêu cuối cùng trải dài từ giá trị nhỏ nhất đến giá trị lớn nhất, với kích thước bước$d$. 
5. Duyệt mảng đã sắp xếp và duyệt từng cặp liền kề$x[i], x[i+1]$, tính toán$(x[i+1] - x[i]) // d - 1$. Điều này đếm xem có bao nhiêu điểm trung gian bị thiếu giữa chúng trong một cấp số cộng hoàn hảo. 
6. Tính tổng tất cả các số đếm này và đưa ra kết quả. 

### Tại sao nó hoạt động 

Sau khi chúng tôi sửa mảng đã sắp xếp, mọi cấu hình cuối cùng hợp lệ đều phải căn chỉnh tất cả các điểm vào một mạng đồng nhất. Khoảng cách giữa hai điểm gốc liên tiếp bất kỳ trong chuỗi cuối cùng phải là ước số chung của tất cả các khoảng trống quan sát được trong đầu vào được sắp xếp. Ước chung lớn nhất là bước lớn nhất và việc sử dụng nó sẽ giảm thiểu số lượng điểm được yêu cầu vì các bước lớn hơn tương ứng với ít vị trí trung gian hơn. Mỗi khoảng trống sau đó được phân tách chính xác thành các đoạn có kích thước bằng nhau$d$, do đó việc đếm các điểm còn thiếu trở nên xác định và mang tính cục bộ, đảm bảo tính nhất quán trên toàn bộ dây chuyền. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import gcd

n = int(input())
a = list(map(int, input().split()))
a.sort()

d = 0
for i in range(1, n):
    d = gcd(d, a[i] - a[i - 1])

if d == 0:
    print(0)
    sys.exit()

ans = 0
for i in range(n - 1):
    ans += (a[i + 1] - a[i]) // d - 1

print(ans)
```Giải pháp bắt đầu bằng cách sắp xếp tọa độ sao cho tất cả các lý luận về cấu trúc được thực hiện theo một thứ tự nhất quán. Bước tích lũy gcd dần dần hợp nhất tất cả những khác biệt cục bộ thành một kích thước bước toàn cầu duy nhất. Việc kiểm tra số 0 xử lý trường hợp suy biến trong đó tất cả các điểm trùng nhau, mặc dù bài toán đảm bảo các giá trị khác nhau nên nó chủ yếu mang tính phòng thủ. 

Vòng lặp cuối cùng tái tạo lại số điểm cách đều nhau phải nằm giữa mỗi cặp liền kề. biểu thức$(a[i+1] - a[i]) // d - 1$đếm trực tiếp các điểm mạng bị thiếu giữa hai vị trí đã biết theo cấp số cộng được xác định theo bước$d$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
-5 10 5
```Mảng được sắp xếp:$[-5, 5, 10]$Sự khác biệt:$10, 5$GCD:$5$Bây giờ chúng tôi tính toán các điểm còn thiếu: 

| tôi | một [tôi] | a[i+1] | khác biệt | khác biệt / d | mất tích | 
| --- | --- | --- | --- | --- | --- | 
| 0 | -5 | 5 | 10 | 2 | 1 | 
| 1 | 5 | 10 | 5 | 1 | 0 | 

Tổng số thiếu = 1 

Điều này xác nhận rằng việc chèn 0 sẽ tạo ra tiến trình bước 5 hoàn hảo. 

### Ví dụ 2 

đầu vào:```
4
0 3 6 12
```Mảng được sắp xếp: giống nhau 

Sự khác biệt: 3, 3, 6 

GCD = 3 

| tôi | một [tôi] | a[i+1] | khác biệt | khác biệt / d | mất tích | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 3 | 3 | 1 | 0 | 
| 1 | 3 | 6 | 3 | 1 | 0 | 
| 2 | 6 | 12 | 6 | 2 | 1 | 

Tổng số thiếu = 1 

Điều này cho thấy chỉ cần một điểm trung gian (9). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp chiếm ưu thế, gcd và scan là tuyến tính | 
| Không gian |$O(n)$| lưu trữ mảng đầu vào | 

Thuật toán có hiệu quả đối với$n \le 100{,}000$vì việc sắp xếp và một đường chuyền tuyến tính duy nhất vừa vặn thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    input = sys.stdin.readline
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    from math import gcd

    d = 0
    for i in range(1, n):
        d = gcd(d, a[i] - a[i - 1])

    ans = 0
    for i in range(n - 1):
        ans += (a[i + 1] - a[i]) // d - 1

    return str(ans)

# provided sample
assert run("3\n-5 10 5\n") == "1"

# already uniform spacing
assert run("3\n0 2 4\n") == "0"

# large gap with one insertion
assert run("4\n0 3 6 12\n") == "1"

# negative coordinates
assert run("3\n-10 -4 2\n") == "2"

# already arithmetic progression
assert run("5\n1 4 7 10 13\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 -5 10 5 | 1 | gcd cơ bản và chèn đơn | 
| 3 0 2 4 | 0 | đã tiến triển đồng đều | 
| 4 0 3 6 12 | 1 | kích thước khoảng cách hỗn hợp | 
| 3 -10 -4 2 | 2 | xử lý tọa độ âm | 
| 5 1 4 7 10 13 | 0 | cấp số học hoàn hảo | 

## Vỏ cạnh 

Đối với các điểm đã cách đều nhau như:```
5
1 4 7 10 13
```sự khác biệt được sắp xếp đều là 3, vì vậy gcd vẫn là 3. Mỗi khoảng cách đều thỏa mãn$(a[i+1] - a[i]) // 3 - 1 = 0$, tạo ra đầu ra đúng bằng 0. 

Đối với trường hợp thứ tự ban đầu không đồng đều:```
3
10 0 5
```sắp xếp mang lại$[0, 5, 10]$. Gcd là 5 và không cần thêm phần chèn nào vì thuật toán bỏ qua hoàn toàn thứ tự đầu vào và xây dựng lại cấu trúc từ hình học được sắp xếp. 

Đối với những trường hợp có khoảng trống lớn như:```
2
0 1000000000
```gcd bằng chính sự khác biệt, do đó không cần điểm trung gian. Thuật toán tránh chèn một cách chính xác một lưới dày đặc đầy đủ, vì kích thước bước được tối đa hóa theo ràng buộc rằng tất cả các điểm ban đầu phải nằm trên tiến trình.
