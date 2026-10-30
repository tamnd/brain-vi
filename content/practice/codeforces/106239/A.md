---
title: "CF 106239A - \u6781\u5dee\u6700\u5927\u7684\u533a\u95f4"
description: "Chúng ta được cấp một dãy số nguyên và được yêu cầu chọn một đoạn liền kề sao cho chênh lệch giữa phần tử lớn nhất và nhỏ nhất bên trong đoạn đó càng lớn càng tốt."
date: "2026-06-19T16:26:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106239
codeforces_index: "A"
codeforces_contest_name: "2025\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66\u65b0\u751f\u8d5b(\u51b3\u8d5b)"
rating: 0
weight: 106239
solve_time_s: 56
verified: true
draft: false
---

[CF 106239A - \u6781\u5dee\u6700\u5927\u7684\u533a\u95f4](https://codeforces.com/problemset/problem/106239/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy số nguyên và được yêu cầu chọn một đoạn liền kề sao cho chênh lệch giữa phần tử lớn nhất và nhỏ nhất bên trong đoạn đó càng lớn càng tốt. Cùng với giá trị của chênh lệch tối đa này, chúng ta cũng phải xuất ra một phân khúc đạt được giá trị đó. 

Đầu vào là một mảng có độ dài lên tới 100, với mỗi giá trị từ 0 đến 100. Câu trả lời hợp lệ là bất kỳ cặp chỉ số nào$l, r$sao cho mảng con$A[l..r]$có giá trị lớn nhất có thể là$\max(A[l..r]) - \min(A[l..r])$. 

Ràng buộc$n \le 100$đã báo hiệu rằng một giải pháp bậc ba hoặc thậm chí bậc hai có thể chấp nhận được. Một giải pháp tuyến tính hoặc tuyến tính cũng sẽ ổn, nhưng không cần thiết. Quan sát quan trọng là vấn đề không yêu cầu một cấu trúc duy nhất như thuộc tính đơn điệu đặc biệt hoặc trạng thái động, mà chỉ yêu cầu tối ưu toàn cục trên tất cả các mảng con. 

Một điểm tinh tế là phân khúc tối ưu không nhất thiết phải là duy nhất và nhiều phân khúc có thể đạt được mức chênh lệch tối đa như nhau. Bất kỳ một phân đoạn hợp lệ nào cũng có thể được chấp nhận, điều đó có nghĩa là chúng ta không cần các quy tắc ràng buộc ngoài tính chính xác. 

Một sai lầm ngây thơ nhưng phổ biến là cho rằng phân đoạn tối ưu phải bao gồm mức tối thiểu toàn cục hoặc mức tối đa toàn cục của toàn bộ mảng. Điều đó đúng cho vấn đề này nhưng cần biện minh thông qua logic liệt kê hơn là giả định. 

Ví dụ, hãy xem xét:```
A = [2, 3, 4, 1]
```Phân đoạn tốt nhất là toàn bộ mảng, cho tối đa 4 và tối thiểu 1, chênh lệch 3. Sẽ có một sai lầm khi chọn một phân đoạn như [2, 3, 4], có chênh lệch 2, thiếu thực tế là việc mở rộng để bao gồm 1 sẽ cải thiện kết quả. 

Một trường hợp cạnh khác là khi tất cả các phần tử đều bằng nhau:```
A = [5, 5, 5]
```Mọi phân đoạn đều có chênh lệch 0, vì vậy bất kỳ phân đoạn phần tử đơn lẻ nào cũng hợp lệ. Một giải pháp đơn giản vẫn có thể trả về một đoạn dài hơn một cách không cần thiết, nhưng tính chính xác không bị ảnh hưởng. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là liệt kê mọi mảng con có thể. Với mỗi cặp chỉ số$l, r$, chúng tôi quét mảng con để tính giá trị tối thiểu và tối đa của nó, sau đó cập nhật câu trả lời tốt nhất nếu sự khác biệt này cải thiện giá trị tốt nhất hiện tại. 

Điều này hiệu quả vì nó đánh giá rõ ràng hàm mục tiêu cho mọi phân khúc ứng viên. Tính đúng đắn là ngay lập tức: không có mảng con nào bị bỏ sót. 

Tuy nhiên, chi phí cao. có$O(n^2)$mảng con và tính toán tối thiểu và tối đa cho mỗi lần thực hiện$O(n)$, dẫn đến$O(n^3)$tổng số hoạt động. Trong khi$n = 100$làm cho ranh giới này nhưng vẫn có thể chấp nhận được, nó chậm một cách không cần thiết. 

Chúng ta có thể cải thiện điều này bằng cách sử dụng lại tính toán. Nếu chúng ta sửa điểm cuối bên trái$l$, chúng ta có thể mở rộng điểm cuối phù hợp$r$tăng dần trong khi duy trì mức tối thiểu và tối đa hiện tại trong thời gian không đổi. Mỗi bản mở rộng cập nhật câu trả lời trong$O(1)$, giảm độ phức tạp tổng thể xuống$O(n^2)$. 

Ý tưởng chính là việc tính lại tối thiểu và tối đa từ đầu cho mọi phân đoạn là không cần thiết. Vì các phân đoạn có cùng điểm cuối bên trái tạo thành một chuỗi ngày càng phát triển nên chúng tôi có thể duy trì trạng thái khi mở rộng. 

Điều này biến vấn đề thành một vòng lặp hai lớp trong đó vòng lặp bên ngoài sửa phần đầu và vòng lặp bên trong mở rộng phần cuối trong khi vẫn duy trì cực trị đang chạy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Chấp nhận được nhưng chậm | 
| Mở rộng bằng cách chạy min/max | O(n²) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo các biến để lưu trữ câu trả lời tốt nhất được tìm thấy cho đến nay: chênh lệch tốt nhất, chỉ mục bên trái tốt nhất và chỉ mục bên phải tốt nhất. Chúng tôi bắt đầu với một sự khác biệt rất nhỏ tốt nhất để bất kỳ phân khúc thực tế nào cũng có thể cải thiện nó. 
2. Sửa điểm cuối bên trái$l$từ 1 đến$n$. Điều này thể hiện sự bắt đầu của một mảng con ứng cử viên. Mọi mảng con hợp lệ phải bắt đầu ở đâu đó, vì vậy chúng tôi thử tất cả các khả năng một cách có hệ thống. 
3. Đối với mỗi$l$, khởi tạo hai biến`cur_min`Và`cur_max`ĐẾN$A[l]$. Điều này thể hiện trạng thái của mảng con chỉ bao gồm phần tử duy nhất tại vị trí$l$. 
4. Mở rộng điểm cuối phù hợp$r$từ$l$ĐẾN$n$, đang cập nhật`cur_min = min(cur_min, A[r])`Và`cur_max = max(cur_max, A[r])`. Điều này duy trì cực trị chính xác của mảng con hiện tại mà không cần tính toán lại. 
5. Tính chênh lệch hiện tại`cur_max - cur_min`. Nếu giá trị này lớn hơn mức chênh lệch tốt nhất cho đến nay, hãy cập nhật câu trả lời được lưu trữ thành$(l, r)$và sự khác biệt mới. 
6. Tiếp tục mở rộng cho đến khi tất cả$r$giá trị cho việc này$l$được xử lý rồi chuyển sang bước tiếp theo$l$. 
7. Sau tất cả các lần lặp, xuất ra phân đoạn tốt nhất được lưu trữ và sự khác biệt của nó. 

### Tại sao nó hoạt động 

Ở mỗi bước mở rộng$r$, thuật toán duy trì chính xác mức tối thiểu và tối đa của mảng con hiện tại$[l, r]$. Đây là một bất biến được bảo toàn theo quy tắc cập nhật: việc thêm một phần tử chỉ yêu cầu so sánh nó với cực trị trước đó. Vì mỗi mảng con được biểu diễn duy nhất bởi một cặp nào đó$(l, r)$và mỗi cặp như vậy được đánh giá chính xác một lần, thuật toán sẽ xem xét tất cả các ứng cử viên mà không bị trùng lặp hoặc bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))
    
    best_diff = -1
    best_l, best_r = 0, 0
    
    for l in range(n):
        cur_min = a[l]
        cur_max = a[l]
        
        for r in range(l, n):
            if r > l:
                cur_min = min(cur_min, a[r])
                cur_max = max(cur_max, a[r])
            
            diff = cur_max - cur_min
            if diff > best_diff:
                best_diff = diff
                best_l = l
                best_r = r
    
    print(best_l + 1, best_r + 1, best_diff)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện chiến lược liệt kê hai lớp. Vòng lặp bên ngoài cố định ranh giới bên trái và vòng lặp bên trong mở rộng ranh giới bên phải trong khi vẫn duy trì các giá trị tối thiểu và tối đa đang chạy. Cần phải thay đổi +1 ở đầu ra vì sự cố sử dụng lập chỉ mục dựa trên 1 trong khi mảng Python dựa trên 0. 

Một cạm bẫy triển khai phổ biến là việc khởi tạo lại`cur_min`Và`cur_max`không chính xác bên trong vòng lặp bên trong, điều này sẽ phá hủy tối ưu hóa gia tăng. Một vấn đề tinh vi khác là cập nhật cực trị trước khi tính toán chênh lệch, việc này phải diễn ra theo đúng thứ tự để đảm bảo phần tử hiện tại được đưa vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
2 3 4 1
```| tôi | r | cur_min | cur_max | khác biệt | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 2 | 0 | (1,1,0) | 
| 0 | 1 | 2 | 3 | 1 | (1,2,1) | 
| 0 | 2 | 2 | 4 | 2 | (1,3,2) | 
| 0 | 3 | 1 | 4 | 3 | (1,4,3) | 

Dấu vết này cho thấy việc kéo dài khoảng thời gian cuối cùng sẽ nắm bắt được cả cực trị tổng thể 4 và cực trị 1 như thế nào, tạo ra sự khác biệt tối đa có thể có. 

### Ví dụ 2 

đầu vào:```
5
1 5 3 5 2
```| tôi | r | cur_min | cur_max | khác biệt | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 0 | (1,1,0) | 
| 0 | 1 | 1 | 5 | 4 | (1,2,4) | 
| 0 | 4 | 1 | 5 | 4 | (1,2,4) | 
| 2 | 4 | 2 | 5 | 3 | (1,2,4) | 

Các phân đoạn tối ưu ở đây là nhiều phân đoạn, bao gồm [1,2] và [1,4], cả hai đều cho điểm khác biệt 4. Thuật toán giữ chính xác phân đoạn tốt nhất được tìm thấy đầu tiên nhưng cũng sẽ chấp nhận các phân đoạn khác nếu chúng xuất hiện sau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Hai vòng lặp lồng nhau trên tất cả các điểm cuối của mảng con, với các cập nhật O(1) mỗi bước | 
| Không gian | O(1) | Chỉ có một số biến vô hướng được duy trì | 

Với$n \le 100$, tổng số lần lặp nhiều nhất là 10.000, con số này không đáng kể trong các giới hạn thông thường. Ngay cả giải pháp bậc ba cũng có thể được chấp nhận, nhưng phiên bản bậc hai thì rõ ràng và hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n = int(sys.stdin.readline().strip())
    a = list(map(int, sys.stdin.readline().split()))
    
    best_diff = -1
    best_l, best_r = 0, 0
    
    for l in range(n):
        cur_min = a[l]
        cur_max = a[l]
        
        for r in range(l, n):
            if r > l:
                cur_min = min(cur_min, a[r])
                cur_max = max(cur_max, a[r])
            
            diff = cur_max - cur_min
            if diff > best_diff:
                best_diff = diff
                best_l = l
                best_r = r
    
    return f"{best_l+1} {best_r+1} {best_diff}"

# provided sample-style cases
assert run("4\n2 3 4 1\n") == "1 4 3"

# minimum size
assert run("1\n7\n") == "1 1 0"

# all equal
assert run("3\n5 5 5\n") == "1 1 0"

# increasing sequence
assert run("5\n1 2 3 4 5\n") == "1 5 4"

# decreasing sequence
assert run("4\n9 7 5 3\n") == "1 4 6"

# mixed
assert run("5\n1 5 3 5 2\n") == "1 2 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 1 0 | độ chính xác kích thước tối thiểu | 
| tất cả đều bình đẳng | 1 1 0 | xử lý phương sai bằng không | 
| ngày càng tăng | 1 5 4 | khoảng tối thiểu/tối đa toàn cầu | 
| giảm dần | 1 4 6 | trật tự độc lập | 
| hỗn hợp | 1 2 4 | phát hiện mảng con tối ưu sớm | 

## Vỏ cạnh 

Đối với mảng một phần tử như`A = [7]`, bộ thuật toán`cur_min = cur_max = 7`và ngay lập tức đánh giá chênh lệch bằng 0. Vì không tồn tại phân đoạn nào khác nên nó xuất ra một cách chính xác`(1, 1, 0)`. 

Đối với một mảng hoàn toàn bằng nhau như`A = [3, 3, 3, 3]`, mỗi lần lặp lại duy trì`cur_min = cur_max = 3`bất kể việc mở rộng. Sự khác biệt không bao giờ vượt quá 0, vì vậy đoạn đầu tiên`(1, 1)`vẫn là câu trả lời được lưu trữ. 

Đối với một mảng tăng nghiêm ngặt như`A = [1, 2, 3, 4]`, vòng lặp bên ngoài đầu tiên đã phát hiện ra rằng việc mở rộng đến toàn bộ phạm vi sẽ tạo ra sự khác biệt tối đa. Bất biến đảm bảo rằng một khi cả hai cực được bao gồm, không phân đoạn nào sau đó có thể vượt quá giá trị đó, vì tất cả các mảng con trong tương lai đều là tập hợp con hoặc các biến thể có phạm vi bằng nhau.
