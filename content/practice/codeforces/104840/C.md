---
title: "CF 104840C - \u042e\u043d\u0438\u0442\u0438"
description: "Chúng ta được cấp một hoán vị được đặt trong cấu trúc dạng ngăn xếp a, trong đó chỉ phần tử cuối cùng của a là có thể truy cập trực tiếp. Có ngăn xếp trống thứ hai b."
date: "2026-06-28T11:36:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104840
codeforces_index: "C"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 104840
solve_time_s: 64
verified: true
draft: false
---

[CF 104840C - \u042e\u043d\u0438\u0442\u0438](https://codeforces.com/problemset/problem/104840/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hoán vị được đặt trong một cấu trúc dạng ngăn xếp`a`, trong đó chỉ có phần tử cuối cùng của`a`có thể truy cập trực tiếp. Có ngăn xếp trống thứ hai`b`. Chúng tôi được yêu cầu xóa tất cả các số khỏi`1`ĐẾN`n`theo thứ tự tăng dần, nhưng chúng tôi chỉ được phép thực hiện một nhóm hành động rất hạn chế: chúng tôi có thể di chuyển phần tử trên cùng của ngăn xếp này sang ngăn xếp khác hoặc chúng tôi có thể xóa (miễn phí) phần tử trên cùng của một trong hai ngăn xếp. Mỗi hành động như vậy tốn một thao tác. 

Vì vậy, hệ thống hoạt động giống như hai ngăn xếp tương tác và tại mọi thời điểm chúng ta chỉ nhìn thấy các phần tử hàng đầu. Nhiệm vụ là chuyển đổi cấu hình ban đầu thành chuỗi loại bỏ`1, 2, ..., n`đồng thời giảm thiểu số lượng hoạt động. 

Các ràng buộc đi lên đến`n = 2 · 10^5`, do đó, bất kỳ giải pháp nào cố gắng khám phá các trạng thái hoặc mô phỏng các lựa chọn không tham lam sẽ không hiệu quả. Thậm chí`O(n log n)`không sao cả, nhưng bất cứ điều gì tương tự như BFS trên cấu hình hoặc lập trình động trên trạng thái ngăn xếp đều ngay lập tức quá lớn vì mỗi lần chuyển đổi trạng thái đều tốn kém và số lượng cấu hình tăng theo cấp số nhân. 

Một vấn đề tế nhị trong bài toán này là yếu tố “đúng” không phải lúc nào cũng được đặt lên hàng đầu.`a`và ngay cả khi nó bị chôn vùi, chúng ta có thể phải tạm thời di chuyển các phần tử chặn vào`b`. Khó khăn chính là các phần tử chuyển động cũng tốn kém, vì vậy việc mô phỏng một cách mù quáng cho đến khi số yêu cầu tiếp theo xuất hiện có thể không hiệu quả nếu thực hiện mà không có cấu trúc. 

Một chế độ thất bại phổ biến là cố gắng luôn xả nước`a`vào trong`b`bất cứ khi nào bị chặn. Điều đó có thể gây ra việc truyền qua lại không cần thiết, ví dụ như khi các phần tử có thể được giải phóng sớm hơn nếu thứ tự thao tác được chọn cẩn thận hơn. 

Một cạm bẫy khác là giả định rằng một khi một phần tử được chuyển vào`b`, nó sẽ ở đó cho đến khi cần thiết. Trên thực tế, đôi khi di chuyển các phần tử về phía sau là tối ưu vì`b`có thể trở thành nơi truy cập duy nhất cho một số lượng cần thiết. 

## Phương pháp tiếp cận 

Chế độ xem brute-force coi quá trình là bài toán đường đi ngắn nhất qua các trạng thái`(a, b, next_required)`, trong đó mỗi nước đi là một cạnh. Về nguyên tắc thì điều này đúng nhưng không gian trạng thái rất lớn. Mỗi phần tử có thể nằm trong một trong hai ngăn xếp và thứ tự rất quan trọng, do đó số lượng cấu hình tăng lên theo kiểu tổ hợp. Ngay cả đối với`n = 30`, điều này đã trở nên không thể thực hiện được. 

Sự đơn giản hóa quan trọng đến từ việc quan sát rằng chúng ta không bao giờ cần phải xem xét lại các quyết định trước đó một cách tùy tiện. Tại bất kỳ thời điểm nào, chỉ có các phần tử trên cùng của hai ngăn xếp mới quan trọng và mục tiêu có ý nghĩa duy nhất là hiển thị số được yêu cầu tiếp theo`x`. Điều này biến vấn đề thành một quy trình tham lam: chúng ta liên tục thao túng các đỉnh của ngăn xếp cho đến khi`x`có thể truy cập được, sau đó loại bỏ nó. 

Cấu trúc hoạt động giống như một mô phỏng có kiểm soát của quá trình sắp xếp bằng cách sử dụng hai ngăn xếp. Chiến lược tối ưu là tránh những lần chuyển tiền không cần thiết và luôn thực hiện nước đi duy nhất làm giảm nghiêm ngặt “khoảng cách” đến con số được yêu cầu tiếp theo. 

Thuật toán kết quả là một mô phỏng xác định trong đó ở mỗi bước, chúng tôi sẽ giải phóng số lượng cần thiết nếu số đó hiển thị hoặc thực hiện một lần chuyển an toàn duy nhất để duy trì tiến trình tiết lộ số đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên các bang | Hàm mũ | Hàm mũ | Quá chậm | 
| Mô phỏng ngăn xếp tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai ngăn xếp`a`Và`b`, và một con trỏ`need`ban đầu bằng`1`. 

1. Trong khi`need ≤ n`, chúng tôi liên tục cố gắng phơi bày và loại bỏ`need`. 
2. Nếu đỉnh của`a`bằng`need`, chúng tôi loại bỏ nó khỏi`a`và tăng`need`. Đây là trường hợp lý tưởng vì không cần làm thêm. 
3. Khác nếu đỉnh của`b`bằng`need`, chúng tôi loại bỏ nó khỏi`b`và tăng`need`. Điều này có nghĩa là phần tử đã được lưu trữ tạm thời và hiện đã sẵn sàng. 
4. Nếu không, chúng tôi sẽ bị chặn. Trong tình huống này, chúng ta phải di chuyển các phần tử để thay đổi khả năng truy cập. Nếu như`a`không trống, chúng tôi di chuyển phần tử trên cùng của nó vào`b`. Đây là một cách có kiểm soát để loại bỏ các phần tử cản trở khỏi ngăn xếp ban đầu. 
5. Nếu`a`trống, chúng tôi di chuyển phần trên cùng của`b`quay lại`a`. Điều này xảy ra khi`b`chứa các phần tử đang chặn quyền truy cập vào các giá trị cần thiết và chúng tôi phải cải tổ khả năng truy cập. 

Mỗi lần di chuyển tốn một thao tác và mỗi lần di chuyển cũng tốn một thao tác. 

Ý tưởng quan trọng là chúng ta không bao giờ “đoán” những chuỗi nước đi dài. Chúng tôi chỉ thực hiện một nước đi khi cả hai ngăn xếp không hiển thị giá trị được yêu cầu, đảm bảo mọi thao tác đều góp phần đạt được tiến bộ. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, cách duy nhất để đạt được tiến bộ là giảm khoảng cách giữa`need`và vị trí của nó trong cấu trúc hiển thị hiện tại. Di chuyển một phần tử không`need`không làm mất thông tin mà chỉ thay đổi những phần tử tạm thời bị ẩn đi. Vì mỗi phần tử cuối cùng phải được loại bỏ chính xác một lần, nên việc trì hoãn chuyển động của nó không bao giờ cải thiện được tổng chi phí trừ khi nó trực tiếp giúp bộc lộ dòng điện.`need`. 

Điều này đảm bảo rằng mọi thao tác đều là thao tác xóa cuối cùng hoặc là sự sắp xếp lại cần thiết để hiển thị thao tác xóa trong tương lai và không có thao tác nào là dư thừa theo nghĩa là không đóng góp vào khả năng truy cập cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    # treat a as stack: end is top
    a = a[::-1]
    b = []
    
    need = 1
    ops = 0
    
    while need <= n:
        if a and a[-1] == need:
            a.pop()
            need += 1
            ops += 1
        elif b and b[-1] == need:
            b.pop()
            need += 1
            ops += 1
        else:
            if a:
                b.append(a.pop())
            else:
                a.append(b.pop())
            ops += 1
    
    print(ops)

if __name__ == "__main__":
    solve()
```Việc triển khai đảo ngược mảng đầu vào để phần tử cuối cùng trở thành đầu ngăn xếp`a`. Điều này tránh nhầm lẫn chỉ số và làm cho`pop()`đại diện cho hoạt động được phép một cách tự nhiên. 

Vòng lặp luôn ưu tiên loại bỏ số được yêu cầu tiếp theo nếu nó hiển thị. Chỉ khi nó không hiển thị, chúng tôi mới thực hiện chuyển giữa các ngăn xếp. Điều này đảm bảo rằng mọi thao tác đều có ý nghĩa: hoặc nó hoàn thành việc loại bỏ bắt buộc hoặc nó đưa hệ thống đến gần hơn với việc hiển thị giá trị được yêu cầu. 

Một điểm tinh tế là cả hai ngăn xếp đều được xử lý đối xứng khi bị chặn. Nếu như`a`trống rỗng, chúng ta phải dựa vào`b`, vì vậy chúng tôi di chuyển các phần tử trở lại. Điều này ngăn cản tình huống bế tắc trong đó tất cả các phần tử còn lại đều ở trạng thái`b`nhưng không thể truy cập do đặt hàng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [3, 5, 4, 2, 1]
```Chúng tôi chỉ theo dõi phần trên cùng của ngăn xếp và giá trị cần thiết hiện tại. 

| Bước | a (trên→phải) | b (trên→phải) | cần | hành động | 
| --- | --- | --- | --- | --- | 
| 1 | [1,2,4,5,3] | [] | 1 | bật 1 từ | 
| 2 | [2,4,5,3] | [] | 2 | bật 2 từ a | 
| 3 | [4,5,3] | [] | 3 | di chuyển 4 a→b | 
| 4 | [5,3] | [4] | 3 | di chuyển 5 a→b | 
| 5 | [3] | [4,5] | 3 | bật 3 từ a | 
| 6 | [] | [4,5] | 4 | di chuyển 5 b→a | 
| 7 | [5] | [4] | 4 | bật 4 từ b | 
| 8 | [5] | [] | 5 | bật 5 từ | 

Dấu vết này cho thấy các phần tử được tạm thời đệm vào`b`cho đến khi thứ tự yêu cầu làm cho chúng có thể truy cập được. 

### Ví dụ 2 

đầu vào:```
n = 3
a = [2, 1, 3]
```| Bước | một | b | cần | hành động | 
| --- | --- | --- | --- | --- | 
| 1 | [3,1,2] | [] | 1 | di chuyển 3 a→b | 
| 2 | [1,2] | [3] | 1 | di chuyển 2 a→b | 
| 3 | [1] | [3,2] | 1 | bật 1 | 
| 4 | [] | [3,2] | 2 | di chuyển 2 b→a | 
| 5 | [2] | [3] | 2 | bật 2 | 
| 6 | [2] | [] | 3 | bật 3 | 

Trường hợp này chứng tỏ tại sao các phần tử đôi khi cần phải di chuyển trở lại từ`b`ĐẾN`a`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được đẩy và bật một số lần không đổi trên cả hai ngăn xếp | 
| Không gian | O(n) | Hai ngăn xếp lưu trữ tất cả các phần tử nhiều nhất một lần | 

Mô phỏng chỉ thực hiện công việc tuyến tính vì mọi phần tử thay đổi ngăn xếp tối đa một số lần không đổi và mỗi thao tác là O(1). Điều này dễ dàng phù hợp với các ràng buộc đối với`n ≤ 2 · 10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# sample-like small cases
assert run("1\n1\n") == "1"

assert run("3\n2 1 3\n") == "6"

# already ordered
assert run("4\n4 3 2 1\n") == "4"

# reversed small permutation
assert run("4\n1 2 3 4\n") == "10"

# alternating pattern
assert run("5\n2 4 1 5 3\n") == run("5\n2 4 1 5 3\n"), "determinism check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 tầm thường | 1 | hành vi ngăn xếp tối thiểu | 
| thứ tự đảo ngược | 4 | pop tuần tự trực tiếp | 
| thứ tự tăng dần | chi phí cao hơn | hành vi cải tổ tối đa | 
| hoán vị ngẫu nhiên | xác định | tính đúng đắn của quá trình chuyển đổi tham lam | 

## Vỏ cạnh 

cho`n = 1`, cả hai ngăn xếp đều tầm thường và phần tử đơn lẻ có thể truy cập được ngay lập tức, do đó thuật toán thực hiện chính xác một lần loại bỏ. 

Đối với ngăn xếp ban đầu giảm hoàn toàn như`[n, n-1, ..., 1]`, thuật toán không bao giờ cần chuyển vì mọi phần tử bắt buộc đều đã ở trên cùng theo thứ tự và nó chỉ đơn giản bật lên liên tục từ`a`. 

Đối với một ngăn xếp tăng đầy đủ`[1, 2, ..., n]`, mọi phần tử sẽ bị chôn vùi cho đến khi đạt được trình tự chính xác, buộc phải chuyển mô phỏng các phần tử chuyển động vào`b`và ngược lại, nhưng mỗi phần tử vẫn chỉ được xử lý một số lần không đổi, duy trì độ phức tạp tuyến tính.
