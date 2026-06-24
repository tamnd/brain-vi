---
title: "CF 105272J - Bữa tối của Sao Mộc"
description: "Chúng ta được đưa ra một chuỗi các yêu cầu được sắp xếp thành một dòng, trong đó mỗi vị trí tương ứng với một người đã gọi một loại món ăn cụ thể. Bên cạnh đó, còn có một hạn chế: người phục vụ chỉ có k “tay”, mỗi tay có thể bưng nhiều món ăn tùy ý nhưng chỉ được một loại món ăn."
date: "2026-06-23T14:03:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105272
codeforces_index: "J"
codeforces_contest_name: "IX MaratonUSP Freshman Contest"
rating: 0
weight: 105272
solve_time_s: 48
verified: true
draft: false
---

[CF 105272J - Bữa tối của Sao Mộc](https://codeforces.com/problemset/problem/105272/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một chuỗi các yêu cầu được sắp xếp thành một dòng, trong đó mỗi vị trí tương ứng với một người đã gọi một loại món ăn cụ thể. Bên cạnh đó, còn có một hạn chế: người phục vụ chỉ có k “tay”, mỗi tay có thể bưng nhiều món ăn tùy ý nhưng chỉ được một loại món ăn. Điều này có nghĩa là nếu một khoảng khách hàng có tối đa k loại món ăn riêng biệt thì người phục vụ có thể phục vụ tất cả chúng trong một chuyến. 

Nhiệm vụ là tìm một phân đoạn liền kề của mảng chứa tối đa k giá trị riêng biệt và trong số tất cả các phân đoạn như vậy, chúng ta muốn một phân đoạn có độ dài tối đa có thể. Chúng ta phải xuất ra cả độ dài và chỉ số của một đoạn tối ưu. 

Độ dài mảng và k có thể lớn tới 200000, do đó, mọi cách tiếp cận bậc hai hoặc gần bậc hai sẽ không thành công. Một giải pháp phải xử lý mảng theo thời gian tuyến tính hoặc gần tuyến tính, thường là O(n) hoặc O(n log n). Điều này gợi ý rõ ràng về kỹ thuật cửa sổ trượt hoặc hai con trỏ với tính năng theo dõi tần số. 

Một cách tiếp cận đơn giản sẽ thử tất cả các mảng con và đếm các phần tử riêng biệt trong mỗi mảng. Điều đó dẫn đến các mảng con O(n²) và ngay cả khi tính toán được tối ưu hóa, nó vẫn trở nên quá chậm vì việc duy trì số lượng riêng biệt vẫn khiến chi phí khấu hao O(1) nhưng số lượng mảng con quá lớn. 

Một trường hợp thất bại tinh vi hơn đối với lý luận ngây thơ xuất hiện khi mảng có nhiều khối lặp lại. Ví dụ: nếu tất cả các phần tử đều khác biệt và k nhỏ thì hầu hết các khoảng đều không hợp lệ, nhưng chúng ta vẫn sẽ lãng phí thời gian để kiểm tra chúng. Ngược lại, nếu k lớn thì nhiều khoảng có giá trị và lực lượng vũ phu lại bùng nổ. 

Khó khăn cốt lõi không phải là kiểm tra tính hợp lệ mà là duy trì hiệu quả tập hợp các phần tử riêng biệt trong khi di chuyển qua tất cả các khoảng ứng cử viên. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: chọn mọi điểm cuối bên trái l, mở rộng r từ l đến n và theo dõi xem có bao nhiêu giá trị riêng biệt trong phân đoạn hiện tại. Mỗi lần mở rộng r, chúng tôi cập nhật bảng tần số. Nếu số lượng phần tử riêng biệt nhiều nhất là k, chúng tôi sẽ cập nhật câu trả lời tốt nhất. Điều này đúng vì nó khám phá tất cả các khoảng có thể. 

Tuy nhiên, số khoảng là O(n2). Mặc dù tần suất cập nhật khi mở rộng r là O(1), chúng tôi vẫn thực hiện khoảng n(n+1)/2 mở rộng, quá lớn đối với n lên tới 200000. 

Quan sát quan trọng là ràng buộc “nhiều nhất k giá trị riêng biệt” là đơn điệu đối với các khoảng thu nhỏ và mở rộng. Nếu một khoảng hợp lệ, việc mở rộng nó có thể phá vỡ tính hợp lệ và nếu nó không hợp lệ, việc thu nhỏ nó có thể khôi phục lại tính hợp lệ. Cấu trúc này chính xác là thứ cho phép tiếp cận cửa sổ trượt hai con trỏ. 

Thay vì tính toán lại từ đầu cho mỗi l, chúng tôi duy trì một cửa sổ [l, r] và đảm bảo nó luôn thỏa mãn ràng buộc. Chúng ta di chuyển r về phía trước một cách tham lam và bất cứ khi nào ràng buộc bị vi phạm, chúng ta sẽ di chuyển l về phía trước cho đến khi nó trở lại hợp lệ. Mỗi chỉ mục vào và ra khỏi cửa sổ nhiều nhất một lần, tạo ra độ phức tạp tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Hai con trỏ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì bản đồ tần số của các giá trị bên trong cửa sổ hiện tại và bộ đếm theo dõi số lượng giá trị riêng biệt hiện có. Chúng tôi cũng duy trì hai con trỏ l và r xác định phân đoạn hiện tại và các biến theo dõi phân đoạn tốt nhất được thấy cho đến nay.

1. Khởi tạo l = 0, best_length = 0 và cấu trúc tần số trống. Số lượng khác biệt hiện tại là 0 vì cửa sổ trống. 
2. Mở rộng r từ trái sang phải. Đối với mỗi phần tử mới a[r], hãy tăng tần số của nó. Nếu giá trị này trước đây không có trong cửa sổ, hãy tăng bộ đếm riêng biệt. Bước này mở rộng cửa sổ một cách tham lam. 
3. Sau khi chèn a[r], nếu số lượng giá trị phân biệt vượt quá k thì phải khôi phục tính hợp lệ. Chúng tôi làm điều này bằng cách di chuyển l về phía trước. Đối với mỗi bước, chúng tôi giảm tần số a[l]. Nếu nó trở thành 0, chúng ta sẽ giảm bộ đếm riêng biệt. Chúng ta tiếp tục cho đến khi cửa sổ có tối đa k giá trị riêng biệt. 
4. Khi cửa sổ hợp lệ, chúng tôi so sánh độ dài của nó (r - l + 1) với độ dài tốt nhất được tìm thấy cho đến nay. Nếu nó lớn hơn, chúng tôi cập nhật câu trả lời tốt nhất và lưu trữ l và r hiện tại. 
5. Tiếp tục cho đến khi r đến cuối mảng. 

Tại sao điều này lại hiệu quả: tại mỗi r, chúng ta tìm thấy đoạn hợp lệ dài nhất kết thúc tại r. Bất kỳ đoạn ngắn hơn nào kết thúc bằng r đều được chứa bên trong nó, vì vậy nó không thể cải thiện câu trả lời ngoài những gì chúng ta đã xem xét. Việc điều chỉnh trượt đảm bảo rằng l luôn luôn nhỏ nhất sao cho ràng buộc giữ nguyên, do đó cửa sổ luôn là cửa sổ hợp lệ tối đa cho r đó. 

Điều bất biến là khi bắt đầu mỗi lần lặp cho r, cửa sổ [l, r-1] chứa tối đa k giá trị riêng biệt và sau khi xử lý r và điều chỉnh l, cửa sổ [l, r] là ranh giới bên trái nhỏ nhất giữ tối đa k giá trị riêng biệt. Điều này đảm bảo không có phân đoạn hợp lệ nào bị bỏ qua và mọi phân đoạn tối đa ứng cử viên đều được kiểm tra chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    freq = {}
    distinct = 0

    l = 0
    best_len = 0
    best_l = 0
    best_r = 0

    for r in range(n):
        x = a[r]
        if x not in freq or freq[x] == 0:
            freq[x] = 1
            distinct += 1
        else:
            freq[x] += 1

        while distinct > k:
            y = a[l]
            freq[y] -= 1
            if freq[y] == 0:
                distinct -= 1
            l += 1

        if r - l + 1 > best_len:
            best_len = r - l + 1
            best_l = l
            best_r = r

    print(best_len)
    print(best_l + 1, best_r + 1)

if __name__ == "__main__":
    solve()
```Từ điển tần số theo dõi số lần mỗi loại món ăn xuất hiện bên trong cửa sổ hiện tại. Bộ đếm riêng biệt tránh việc quét liên tục các khóa từ điển. Vòng lặp bên trong đảm bảo cửa sổ luôn thỏa mãn ràng buộc k-distinct sau mỗi phần mở rộng của r. 

Việc lập chỉ mục được xử lý cẩn thận bằng cách lưu trữ best_l và best_r ở dạng dựa trên 0 và chuyển đổi thành đầu ra dựa trên một ở cuối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2
1 2 3 2 1
```Chúng tôi theo dõi cửa sổ trượt: 

| r | một [r] | tôi | cửa sổ | khác biệt | hành động | tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | [1] | 1 | hợp lệ | [1,1] | 
| 1 | 2 | 0 | [1,2] | 2 | hợp lệ | [1,2] | 
| 2 | 3 | 0→1 | [2,3] | 2 | thu nhỏ | [1,2] | 
| 3 | 2 | 1 | [2,3,2] | 2 | hợp lệ | [2,4] | 
| 4 | 1 | 1→2 | [3,2,1] | 3→2 | thu nhỏ | [2,4] | 

Phân đoạn tốt nhất là [2,4], phù hợp với cửa sổ tối ưu khi r = 3. 

Dấu vết này cho thấy con trỏ bên trái chỉ di chuyển như thế nào khi ràng buộc bị vi phạm, duy trì các cửa sổ hợp lệ tối đa. 

### Ví dụ 2 

đầu vào:```
8 3
4 1 2 3 3 2 1 4
```| r | một [r] | tôi | cửa sổ | khác biệt | hành động | tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 4 | 0 | [4] | 1 | hợp lệ | [1,1] | 
| 1 | 1 | 0 | [4,1] | 2 | hợp lệ | [1,2] | 
| 2 | 2 | 0 | [4,1,2] | 3 | hợp lệ | [1,3] | 
| 3 | 3 | 0 | [4,1,2,3] | 4→3 | thu nhỏ lại l=1 | [2,4] | 
| 4 | 3 | 1 | [1,2,3,3] | 3 | hợp lệ | [2,5] | 
| 5 | 2 | 1 | [1,2,3,3,2] | 3 | hợp lệ | [2,6] | 
| 6 | 1 | 1 | [1,2,3,3,2,1] | 3 | hợp lệ | [2,7] | 
| 7 | 4 | 1→2 | [2,3,3,2,1,4] | 4→3 | thu nhỏ | [2,7] | 

Phân đoạn tốt nhất trở thành [2,7], minh họa rằng thuật toán ghi lại các đoạn dài ổn định sau khi cửa sổ được giảm để thỏa mãn k. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi chỉ mục vào và ra khỏi cửa sổ nhiều nhất một lần | 
| Không gian | O(n) | bản đồ tần số lưu trữ số lượng phần tử hoạt động | 

Hành vi tuyến tính đủ cho n lên tới 200000 và mức sử dụng bộ nhớ phù hợp thoải mái trong giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    freq = {}
    distinct = 0
    l = 0

    best_len = 0
    best_l = best_r = 0

    for r in range(n):
        x = a[r]
        freq[x] = freq.get(x, 0) + 1
        if freq[x] == 1:
            distinct += 1

        while distinct > k:
            y = a[l]
            freq[y] -= 1
            if freq[y] == 0:
                distinct -= 1
            l += 1

        if r - l + 1 > best_len:
            best_len = r - l + 1
            best_l, best_r = l, r

    return f"{best_len}\n{best_l+1} {best_r+1}\n"

# provided samples
assert run("5 2\n1 2 3 2 1\n") == "3\n2 4\n"
assert run("8 3\n4 1 2 3 3 2 1 4\n") == "6\n2 7\n"

# custom cases
assert run("1 1\n7\n") == "1\n1 1\n"
assert run("5 5\n1 2 3 4 5\n") == "5\n1 5\n"
assert run("5 1\n1 1 1 1 1\n") == "5\n1 5\n"
assert run("6 2\n1 1 2 2 3 3\n") == "4\n1 4\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | đầy đủ | độ chính xác kích thước tối thiểu | 
| k lớn | toàn bộ mảng | giá trị tầm thường | 
| k = 1 | tất cả đều bình đẳng | xử lý ràng buộc nghiêm ngặt | 
| nhóm xen kẽ | một phần cửa sổ | thu nhỏ độ chính xác | 

## Vỏ cạnh 

Trường hợp tối thiểu với n = 1 kiểm tra xem thuật toán có khởi tạo chính xác các câu trả lời tốt nhất mà không cần đi vào vòng lặp rút gọn hay không. Cửa sổ bắt đầu hợp lệ và cần được ghi lại ngay lập tức. 

Trường hợp k ≥ số lượng giá trị riêng biệt kiểm tra xem thuật toán có tránh được việc thu hẹp không cần thiết hay không. Cửa sổ sẽ mở rộng hoàn toàn mà không có bất kỳ sự co lại nào, xác nhận tính đúng đắn của logic “chỉ thu nhỏ khi cần”. 

Trường hợp k = 1 và tất cả các phần tử khác nhau sẽ kiểm tra hành vi thu hẹp mạnh mẽ. Cửa sổ phải luôn thu gọn thành một phần tử duy nhất, đảm bảo bản đồ tần số đặt lại số đếm một cách chính xác và bộ đếm riêng biệt được giảm chính xác khi một giá trị hoàn toàn rời khỏi cửa sổ.
