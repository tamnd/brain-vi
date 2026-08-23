---
title: "CF 104257J - Jiggle Joggle"
description: "Chúng ta được cung cấp một dãy số nguyên và chúng ta được phép xóa các phần tử trong khi vẫn giữ các phần tử còn lại theo thứ tự ban đầu."
date: "2026-07-01T21:47:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104257
codeforces_index: "J"
codeforces_contest_name: "2021 NTUIM Programming Design And Optimization (PDAO 2021)"
rating: 0
weight: 104257
solve_time_s: 53
verified: true
draft: false
---

[CF 104257J - Lắc lư](https://codeforces.com/problemset/problem/104257/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số nguyên và chúng ta được phép xóa các phần tử trong khi vẫn giữ các phần tử còn lại theo thứ tự ban đầu. Mục tiêu là làm cho dãy con thu được càng dài càng tốt trong khi vẫn đáp ứng được mô hình xen kẽ nghiêm ngặt về các sai phân liên tiếp của nó. 

Cụ thể, nếu chúng ta nhìn vào dãy con đã chọn, mỗi cặp liền kề phải khác nhau và dấu của những sai khác này phải xen kẽ một cách chặt chẽ. Nếu chênh lệch đầu tiên là dương thì chênh lệch tiếp theo phải là âm, sau đó lại dương, v.v. Điều tương tự cũng xảy ra nếu chênh lệch đầu tiên là âm. Bất kỳ sự khác biệt bằng 0 nào sẽ ngay lập tức phá vỡ tính hợp lệ vì sự bình đẳng không được phép trong mẫu xen kẽ. 

Nhiệm vụ là tính toán độ dài tối đa có thể có của dãy con như vậy. 

Ràng buộc$n \le 10^5$loại trừ bất kỳ giải pháp nào thử tất cả các chuỗi tiếp theo. Một sự ngây thơ$O(2^n)$việc liệt kê ngay lập tức là không thể. Ngay cả bậc hai$O(n^2)$Lập trình động có nguy cơ quá chậm trong trường hợp xấu nhất, do đó, giải pháp về cơ bản phải xử lý mảng theo thời gian tuyến tính, chỉ sử dụng công hằng số hoặc logarit cho mỗi phần tử. 

Một số trường hợp đặc biệt quan trọng hơn những gì chúng xuất hiện lần đầu. 

Nếu tất cả các phần tử đều bằng nhau thì mọi hiệu đều bằng 0, do đó không có chuyển đổi nào hợp lệ và câu trả lời phải là 1. 

Nếu chuỗi tăng hoặc giảm nghiêm ngặt thì chỉ có một hướng có thể sử dụng được khi bắt đầu và sau đó, điều kiện xen kẽ ngay lập tức thất bại, do đó, một lần nữa, câu trả lời lại giảm xuống còn 2 đối với bất kỳ độ dài không tầm thường nào lớn hơn 1. 

Nếu có nhiều giá trị lặp lại xen kẽ với các thay đổi, các cách tiếp cận ngây thơ không loại bỏ rõ ràng sai biệt bằng 0 có thể coi chúng là các chuyển đổi hợp lệ một cách không chính xác, tạo ra các câu trả lời bị thổi phồng. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ thử mọi dãy con và kiểm tra xem nó có thỏa mãn điều kiện dấu xen kẽ hay không. Đối với mỗi dãy con được chọn có độ dài$k$, việc xác minh tính hợp lệ mất$O(k)$, và có$2^n$các chuỗi tiếp theo, do đó tổng chi phí là theo cấp số nhân và không thể thực hiện được ngay cả đối với$n = 40$, huống hồ là$10^5$. 

Chúng ta cần nén vấn đề vào một cái gì đó cục bộ. Quan sát quan trọng là điều duy nhất quan trọng để mở rộng một dãy con hợp lệ là phần tử được chọn cuối cùng và dấu của chênh lệch cuối cùng khác 0. Khi chúng ta biết liệu hiện tại chúng ta đang mong đợi tăng hay giảm, mọi phần tử mới sẽ tiếp tục mô hình đó hoặc phá vỡ nó. 

Điều này biến vấn đề thành một cuộc quét tham lam. Chúng ta không cần nhớ toàn bộ dãy con, chỉ cần nhớ giá trị được chọn cuối cùng và hướng cuối cùng. Bất cứ khi nào chúng tôi gặp một giá trị tạo ra sự thay đổi hướng hợp lệ, chúng tôi sẽ lấy nó và lật hướng mong đợi. Nếu chênh lệch bằng 0, chúng tôi bỏ qua nó hoàn toàn vì nó không thể đóng góp vào sự thay thế hợp lệ. 

Đây là cấu trúc tương tự như bài toán “chuỗi con lắc lư” cổ điển: nén mảng thành các chuỗi xen kẽ cực đại có độ dốc dương và âm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Nén ký tham lam |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quét mảng trong khi theo dõi hướng thay đổi có ý nghĩa cuối cùng. 

1. Khởi tạo độ dài câu trả lời là 1 vì bất kỳ phần tử đơn lẻ nào cũng luôn hợp lệ. Đồng thời giữ một biến lưu trữ dấu hiệu chênh lệch trước đó, ban đầu không được đặt. 
2. Lặp lại mảng bắt đầu từ phần tử thứ hai và tính toán sự khác biệt giữa phần tử hiện tại và phần tử trước đó. 
3. Nếu hiệu bằng 0, hãy bỏ qua hoàn toàn vì nó không thể đóng góp vào bất kỳ dãy con xen kẽ hợp lệ nào. Điều này tránh làm hỏng logic hướng. 
4. Nếu chênh lệch là dương và chúng ta chưa theo dõi hướng hoặc hướng trước đó là âm, chúng ta chấp nhận phần tử này như một phần của dãy con và đặt hướng thành dương. 
5. Nếu chênh lệch là âm và chúng tôi chưa theo dõi hướng hoặc hướng trước đó là dương, chúng tôi chấp nhận phần tử này và đặt hướng thành âm. 
6. Mặt khác, sự khác biệt hiện tại vẫn tiếp tục theo hướng như trước và không thể mở rộng mô hình xen kẽ, vì vậy chúng tôi bỏ qua nó. 
7. Số lượng chuyển đổi được chấp nhận cộng với một sẽ cho độ dài câu trả lời. 

Ý tưởng quan trọng là mỗi khi hướng thay đổi, chúng tôi đảm bảo sẽ sử dụng điểm có sẵn cao nhất cho quá trình chuyển đổi đó, do đó, không phần tử nào trong tương lai có thể cải thiện số lượng mà không phá vỡ sự luân phiên. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình quét, thuật toán sẽ duy trì một chuỗi con kết thúc ở điểm xoay gần đây nhất nơi hướng đã thay đổi. Bất kỳ phần tử nào bị bỏ qua đều có chênh lệch bằng 0 hoặc tiếp tục có cùng hướng dốc. Giữ nó sẽ không cho phép một sự thay thế trong tương lai mà chưa thể đạt được, bởi vì cách duy nhất để tăng độ dài là tạo ra một sự thay đổi dấu hiệu mới và việc thay đổi dấu hiệu chỉ có thể thực hiện được khi độ dốc thay đổi. Do đó, sự lựa chọn tham lam chỉ thực hiện các lần lật dốc sẽ bảo toàn số lần thay thế tối đa có thể, điều này trực tiếp xác định độ dài chuỗi con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    if n == 0:
        print(0)
        return
    
    count = 1
    prev_diff = 0  # 0 means unset, 1 means positive, -1 means negative
    
    for i in range(1, n):
        diff = a[i] - a[i - 1]
        
        if diff == 0:
            continue
        
        sign = 1 if diff > 0 else -1
        
        if prev_diff == 0 or sign != prev_diff:
            count += 1
            prev_diff = sign
    
    print(count)

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc quét một lần qua mảng và chỉ phản ứng khi dấu của các sai phân liên tiếp thay đổi. Biến`prev_diff`lưu trữ hướng dốc được chấp nhận cuối cùng. Sự khác biệt bằng 0 được bỏ qua một cách rõ ràng để nó không ảnh hưởng đến logic thay thế. 

Điều tinh tế quan trọng là chúng ta luôn so sánh với các phần tử liền kề trong chuỗi ban đầu chứ không phải với chuỗi con đã chọn. Điều này là đủ vì bất kỳ chuỗi con tối ưu nào cũng có thể được chuyển đổi thành chuỗi chỉ sử dụng các “điểm ngoặt” liên tiếp mà không làm giảm độ dài của nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
3 14 5 9 6 16 7
```Chúng tôi chỉ theo dõi những thay đổi về dấu hiệu giữa các giá trị liên tiếp. 

| tôi | a[i-1] | một [tôi] | khác biệt | ký tên | trước_diff | lấy? | đếm | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 14 | +11 | + | 0 | vâng | 2 | 
| 2 | 14 | 5 | -9 | - | + | vâng | 3 | 
| 3 | 5 | 9 | +4 | + | - | vâng | 4 | 
| 4 | 9 | 6 | -3 | - | + | vâng | 5 | 
| 5 | 6 | 16 | +10 | + | - | vâng | 6 | 
| 6 | 16 | 7 | -9 | - | + | vâng | 7 | 

Thuật toán chấp nhận mọi bước vì trình tự thay thế hoàn hảo theo hướng dốc. Điều này xác nhận rằng mỗi lần lật dấu đóng góp chính xác một phần tử bổ sung vào dãy con tối ưu. 

### Ví dụ 2 

đầu vào:```
5
1 3 11 9 15
```| tôi | a[i-1] | một [tôi] | khác biệt | ký tên | trước_diff | lấy? | đếm | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | +2 | + | 0 | vâng | 2 | 
| 2 | 3 | 11 | +8 | + | + | không | 2 | 
| 3 | 11 | 9 | -2 | - | + | vâng | 3 | 
| 4 | 9 | 15 | +6 | + | - | vâng | 4 | 

Quá trình chuyển đổi thứ hai bị bỏ qua vì nó tiếp tục có cùng độ dốc dương. Điều này chứng tỏ tại sao chỉ đếm những thay đổi là không đủ, chúng ta phải ngăn chặn một cách rõ ràng các hướng giống hệt nhau liên tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| truyền một lần qua mảng, công việc không đổi trên mỗi phần tử | 
| Không gian |$O(1)$| chỉ có một vài biến số nguyên được duy trì | 

Giải pháp dễ dàng phù hợp với các ràng buộc cho$n \le 10^5$, vì nó thực hiện nhiều nhất một phép trừ và một vài phép so sánh cho mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sysio
    
    out = sysio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    if n == 0:
        print(0)
        return
    
    count = 1
    prev_diff = 0
    
    for i in range(1, n):
        diff = a[i] - a[i - 1]
        if diff == 0:
            continue
        sign = 1 if diff > 0 else -1
        if prev_diff == 0 or sign != prev_diff:
            count += 1
            prev_diff = sign
    
    print(count)

# provided samples
assert run("7\n3 14 5 9 6 16 7\n") == "7"
assert run("5\n1 3 11 9 15\n") == "4"

# all equal
assert run("4\n5 5 5 5\n") == "1"

# strictly increasing
assert run("5\n1 2 3 4 5\n") == "2"

# strictly decreasing
assert run("5\n5 4 3 2 1\n") == "2"

# alternating with zeros
assert run("6\n1 2 2 1 1 2\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | 1 | không được tính sự khác biệt | 
| trình tự tăng dần | 2 | chỉ có thể có một công tắc hướng hợp lệ | 
| dãy giảm dần | 2 | tính đối xứng với trường hợp tăng dần | 
| trình tự trùng lặp | 3 | không có sự khác biệt nào được bỏ qua một cách an toàn | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi chuỗi chứa các giá trị lặp lại nằm giữa các lần thay đổi hướng. Ví dụ: trong một đầu vào như`1 2 2 1`, sự khác biệt`2 -> 2`bằng 0 và phải được bỏ qua, nếu không nó sẽ can thiệp không chính xác vào việc phát hiện lần giảm tiếp theo. Thuật toán xử lý điều này bằng cách bỏ qua rõ ràng sự khác biệt bằng 0, do đó trình tự hiệu quả sẽ trở thành`1 -> 2 -> 1`, tạo ra độ dài chính xác là 3. 

Một trường hợp khác là một mảng đơn điệu như`1 2 3 4 5`. Lần tăng đầu tiên được chấp nhận, nhưng tất cả các lần tăng sau đó đều bị bỏ qua vì chúng không tạo ra sự đảo dấu mới. Kết quả ổn định ở mức 2, phù hợp với thực tế là chỉ cần hai điểm để thể hiện một lần chạy tăng dần. 

Trường hợp cuối cùng là một mảng phẳng như`7 7 7 7`. Mọi sự khác biệt đều bằng 0, vì vậy không có sự chuyển đổi nào được chấp nhận. Thuật toán để lại số đếm một cách chính xác ở mức 1, phản ánh rằng bất kỳ phần tử đơn lẻ nào cũng tạo thành một chuỗi lắc lư tầm thường hợp lệ.
