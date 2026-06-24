---
title: "CF 105272E - Khai quật thủy ngân"
description: "Đường chân trời của Sao Thủy được mô tả như một dãy núi, mỗi vị trí có độ cao nhất định. Chúng tôi chỉ được phép thực hiện một loại hoạt động: giảm độ cao của bất kỳ ngọn núi nào bằng cách loại bỏ vật liệu."
date: "2026-06-23T14:02:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105272
codeforces_index: "E"
codeforces_contest_name: "IX MaratonUSP Freshman Contest"
rating: 0
weight: 105272
solve_time_s: 45
verified: true
draft: false
---

[CF 105272E - Khai thác thủy ngân](https://codeforces.com/problemset/problem/105272/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đường chân trời của Sao Thủy được mô tả như một dãy núi, mỗi vị trí có độ cao nhất định. Chúng tôi chỉ được phép thực hiện một loại hoạt động: giảm độ cao của bất kỳ ngọn núi nào bằng cách loại bỏ vật liệu. Việc tăng bất kỳ độ cao nào hoặc chuyển vật liệu giữa các ngọn núi đều bị cấm. 

Mục tiêu là sửa đổi cảnh quan để tất cả các ngọn núi đều có cùng độ cao, đồng thời giảm thiểu tổng lượng vật liệu bị loại bỏ. 

Nói một cách cụ thể hơn, chúng ta bắt đầu với một dãy độ cao. Chúng tôi muốn chọn chiều cao đồng đều cuối cùng và giảm mọi phần tử ở trên mức này xuống mức đó. Bất kỳ yếu tố nào dưới mức đó sẽ cần phải được tăng lên, điều này là không thể, do đó việc lựa chọn chiều cao mục tiêu như vậy là không hợp lệ. 

Ràng buộc n 100000 với độ cao lên tới 10000 ngay lập tức gợi ý rằng giải pháp O(n) hoặc O(n log n) là đủ. Bất cứ điều gì liên quan đến việc kiểm tra tất cả các độ cao mục tiêu có thể có một cách ngây thơ theo cách lồng nhau vẫn sẽ ổn nếu được thực hiện cẩn thận vì phạm vi giá trị nhỏ nhưng hóa ra lại không cần thiết. 

Trường hợp cạnh tinh tế xuất hiện khi mảng không cố định. Ví dụ: nếu tất cả các giá trị đều bằng nhau thì không cần loại bỏ và câu trả lời là 0. Nếu có một giá trị rất nhỏ thì giá trị đó sẽ hạn chế mọi thứ khác. 

Đầu vào như:```
5
10 10 10 10 10
```nên trả về 0. 

Đầu vào như:```
3
5 1 5
```buộc chiều cao cuối cùng là 1, vì chúng ta không thể tăng 1 và do đó mọi thứ khác phải giảm xuống. 

Một sự hiểu lầm ngây thơ sẽ là chọn chiều cao trung bình hoặc trung vị, điều này có thể giảm thiểu khoảng cách trong các vấn đề khác, nhưng ở đây bất kỳ mục tiêu nào trên mức tối thiểu đều không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi chiều cao mục tiêu H có thể có từ 1 đến max(ai). Đối với mỗi ứng cử viên H, chúng tôi kiểm tra tính khả thi: mọi phần tử ít nhất phải là H, nếu không chúng tôi sẽ loại bỏ nó. Nếu khả thi, chúng ta tính chi phí là tổng của mức giảm ai − H trên tất cả i. Chúng tôi lấy mức tối thiểu. 

Điều này hiệu quả vì điều kiện đơn giản nhưng tính kém hiệu quả của nó xuất phát từ việc tính toán lại chi phí cho mọi H có thể. Với max(ai) lên tới 10000 và n lên tới 100000, điều này sẽ trở thành khoảng 10^9 thao tác trong trường hợp xấu nhất, đây là chi phí không cần thiết. 

Quan sát quan trọng là tính khả thi buộc H tối đa phải là phần tử tối thiểu của mảng. Bất kỳ lựa chọn nào cao hơn ngay lập tức làm cho giải pháp không thể thực hiện được vì ít nhất một phần tử sẽ cần được tăng lên. 

Khi chúng tôi giới hạn H ở các lựa chọn hợp lệ, chỉ có một ứng cử viên tối đa hóa H: giá trị tối thiểu trong mảng. Vì chi phí giảm khi H tăng nên chúng tôi muốn H hợp lệ lớn nhất có thể, chính xác là phần tử tối thiểu. 

Điều này làm giảm vấn đề tính tổng của tất cả các phần tử và trừ đi n lần giá trị tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên H | O(n * maxA) | O(1) | Quá chậm | 
| Tối ưu (tối thiểu + tổng) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn biểu thị chiều cao thống nhất cuối cùng dưới dạng một giá trị H, sau đó tính toán số lượng cần loại bỏ. 

1. Quét mảng một lần để tính tổng của tất cả các độ cao và chiều cao tối thiểu. Tổng này bao gồm tổng số vật liệu có sẵn và mức tối thiểu xác định mức độ thống nhất khả thi cao nhất. 
2. Đặt chiều cao mục tiêu H thành giá trị nhỏ nhất trong mảng. Đây là chiều cao lớn nhất có thể mà không cần tăng bất kỳ phần tử nào. 
3. Tính tổng lượng vật liệu bị loại bỏ bằng tổng của tất cả các phần tử trừ n nhân với H. Điều này thể hiện việc giảm mọi phần tử xuống H. 
4. Xuất giá trị này. 

### Tại sao nó hoạt động 

Bất kỳ cấu hình cuối cùng hợp lệ nào cũng phải chọn độ cao H không lớn hơn mọi ai, nếu không một số ngọn núi sẽ yêu cầu tăng lên. Điều này buộc H ≤ min(ai). Đối với bất kỳ H nào như vậy, chi phí là sum(ai − H), đơn giản hóa thành biểu thức tuyến tính trong H. Vì tổng là cố định nên việc tối đa hóa H sẽ giảm thiểu chi phí loại bỏ. H khả thi tối đa chính xác là phần tử mảng tối thiểu, vì vậy không có ứng cử viên nào khác có thể làm tốt hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    total = sum(a)
    mn = min(a)
    
    print(total - n * mn)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo công thức dẫn xuất. Việc chăm sóc duy nhất cần thiết là sử dụng một tập hợp tổng và tối thiểu một lượt. Không có nguy cơ tràn trong Python, nhưng trong các ngôn ngữ khác, người ta phải đảm bảo số nguyên 64 bit vì tổng có thể đạt tới 10^9. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
5 1 5
```Chúng tôi theo dõi tổng và tối thiểu: 

| Bước | Mảng | Tổng hợp | Tối thiểu | H | Chi phí | 
| --- | --- | --- | --- | --- | --- | 
| Sau khi quét | 5 1 5 | 11 | 1 | 1 | 11 - 3 = 8 | 

Chiều cao cuối cùng là 1. Mỗi 5 trở thành 1, đóng góp 4 đơn vị loại bỏ mỗi cái, tổng cộng là 8. 

Điều này xác nhận rằng mọi nỗ lực chọn H = 2 trở lên đều không hợp lệ vì phần tử ở giữa sẽ cần phải tăng lên. 

### Ví dụ 2 

đầu vào:```
4
10 10 10 10
```| Bước | Mảng | Tổng hợp | Tối thiểu | H | Chi phí | 
| --- | --- | --- | --- | --- | --- | 
| Sau khi quét | 10 10 10 10 | 40 | 10 | 10 | 40 - 40 = 0 | 

Tất cả các yếu tố đã phù hợp với chiều cao tối ưu. Không cần loại bỏ. 

Điều này cho thấy thuật toán xử lý chính xác các mảng thống nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần để tính tổng và tối thiểu | 
| Không gian | O(1) | Chỉ có một số biến tích lũy được sử dụng | 

Giải pháp này phù hợp một cách thoải mái trong các giới hạn vì n lên tới 100000 và chỉ thực hiện công việc tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    n = int(input())
    a = list(map(int, input().split()))
    return str(sum(a) - n * min(a))

# provided samples
assert run("3\n5 1 5\n") == "8"
assert run("4\n10 10 10 10\n") == "0"

# minimum size
assert run("1\n7\n") == "0"

# already uniform large
assert run("5\n3 3 3 3 3\n") == "0"

# mixed values
assert run("6\n1 2 3 4 5 6\n") == str(21 - 6*1)

# single minimum in middle
assert run("5\n10 3 10 10 10\n") == str(43 - 5*3)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | núi đơn không cần thay đổi | 
| tất cả đều bình đẳng | 0 | không cần xóa | 
| trình tự tăng dần | tính toán | xác định chính xác ràng buộc tối thiểu | 
| rải rác tối thiểu | tính toán | giải pháp ổ đĩa tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng nhất là khi giá trị nhỏ nhất xuất hiện nhiều lần hoặc chỉ một lần. Thuật toán không phụ thuộc vào tần số, chỉ phụ thuộc vào phần tử nhỏ nhất. Đối với đầu vào:```
5
10 3 10 10 10
```Quá trình quét tạo ra tổng = 43 và min = 3. Kết quả tính toán là 43 − 5 × 3 = 28. Mọi phần tử trên 3 đều bị giảm, trong khi phần tử bằng 3 vẫn không thay đổi, xác nhận tính đúng đắn. 

Một trường hợp khác là khi mức tối thiểu nằm ở ranh giới:```
3
1 100 100
```Ở đây H phải bằng 1. Thuật toán giảm cả hai số 100 xuống còn 1, tạo ra chi phí 198. Bất kỳ H nào cao hơn sẽ buộc tăng 1, điều này là bất hợp pháp, do đó, ràng buộc được thực thi ngầm bằng cách chọn mức tối thiểu.
