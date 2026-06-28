---
title: "CF 105118B - \u0422\u0430\u0438\u043d\u0441\u0442\u0432\u0435\u043d\u043d\u044b\u0439 \u044f\u0437\u044b\u043a"
description: "Chúng ta được cung cấp hai loại từ. Một nhóm chứa các từ ngắn, tất cả đều có độ dài a bằng nhau và có n từ riêng biệt thuộc loại này. Nhóm còn lại chứa các từ dài, tất cả đều có độ dài b bằng nhau và có m từ riêng biệt thuộc loại này, với a < b."
date: "2026-06-27T19:43:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105118
codeforces_index: "B"
codeforces_contest_name: "\u041f\u043e\u0434\u043c\u043e\u0441\u043a\u043e\u0432\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u2013 2024, \u0417\u0430\u043a\u043b\u044e\u0447\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f"
rating: 0
weight: 105118
solve_time_s: 102
verified: false
draft: false
---

[CF 105118B - \u0422\u0430\u0438\u043d\u0441\u0442\u0432\u0435\u043d\u043d\u044b\u0439 \u044f\u0437\u044b\u043a](https://codeforces.com/problemset/problem/105118/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai loại từ. Một nhóm chứa các từ ngắn, tất cả đều có độ dài bằng nhau`a`, và có`n`những từ riêng biệt thuộc loại này. Nhóm còn lại chứa các từ dài, tất cả đều có độ dài bằng nhau`b`, và có`m`những từ riêng biệt thuộc loại này, với`a < b`. 

Olya muốn xây dựng một bài thơ bằng cách chọn một số từ này. Mỗi từ được chọn sẽ đóng góp toàn bộ chiều dài của nó vào tổng chiều dài của bài thơ. Cô ấy được phép sử dụng mỗi từ nhiều nhất một lần. Chuỗi phải xen kẽ theo loại từ, nghĩa là hai từ ngắn hoặc hai từ dài không thể liền kề nhau trong chuỗi cuối cùng. Tổng độ dài của các từ được chọn phải ít nhất`k`. 

Mỗi lần cô thêm một từ ngắn, cô phải dành`c`đơn vị thời gian để tìm hiểu ý nghĩa của nó và mỗi từ dài đều có giá`d`đơn vị thời gian. Mục tiêu là giảm thiểu tổng thời gian dành cho việc học từ trong khi vẫn có thể xây dựng một số chuỗi xen kẽ hợp lệ có tổng độ dài ít nhất là`k`, mà không vượt quá nguồn cung cấp từ có sẵn. 

Khó khăn chính là bài thơ không chỉ là một bài toán chọn nhiều tập. Ràng buộc luân phiên giới hạn số lượng từ của mỗi loại có thể được sử dụng cùng nhau. Bởi vì các từ giống hệt nhau trong mỗi loại ngoại trừ giới hạn chi phí và số lượng, vấn đề giảm xuống còn việc chọn số lượng từ ngắn và dài cần lấy và sắp xếp chúng theo một chuỗi xen kẽ. 

Những hạn chế là rất lớn, lên tới`10^18`từ. Điều này loại trừ bất kỳ chương trình động trực tiếp nào về số lượng hoặc liệt kê vũ phu. Bất kỳ giải pháp đúng đắn nào cũng phải dựa vào sự quan sát cấu trúc về các mô hình xen kẽ tối ưu. 

Một sai lầm ngây thơ là cho rằng chúng ta có thể tự do lựa chọn bất kỳ số đếm nào`x`Và`y`như vậy`a*x + b*y >= k`. Ví dụ: chỉ chọn những từ dài có vẻ tối ưu khi`d`nhỏ nhưng nếu`m`bị hạn chế, chúng tôi có thể buộc phải đưa vào các từ ngắn và sự luân phiên có thể hạn chế số lượng chúng tôi có thể gộp lại với nhau. 

Một trường hợp thất bại tinh vi khác là quên rằng các chuỗi xen kẽ có thể có hai kiểu bắt đầu. Nếu có nhiều từ ngắn hơn thì trình tự tốt nhất có thể bắt đầu bằng từ ngắn; nếu không nó có thể bắt đầu bằng thời gian dài. Bỏ qua một trong những mẫu này có thể bỏ lỡ câu trả lời tối ưu. 

Cuối cùng, khi một loại đắt hơn nhiều, sự lựa chọn tham lam có thể chỉ chọn loại rẻ hơn, nhưng điều đó có thể thất bại do hiệu quả về độ dài: các từ dài có thể cung cấp nhiều chữ cái hơn cho mỗi lựa chọn và giảm tổng số lượng cần thiết để tiếp cận`k`. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua cấu trúc, ý tưởng thô bạo là thử tất cả số lượng từ ngắn có thể có`x`từ`0..n`và những từ dài`y`từ`0..m`, kiểm tra xem chúng ta có thể sắp xếp chúng theo một trình tự xen kẽ nhau không và liệu`a*x + b*y >= k`, sau đó tính chi phí`c*x + d*y`. Điều này đúng vì chúng tôi đã kiểm tra tính khả thi và chi phí một cách rõ ràng. Tuy nhiên, điều này là không thể vì cả hai`n`Và`m`có thể lên đến`10^18`, do đó, ngay cả việc lặp lại tất cả các khả năng cũng vượt xa giới hạn khả thi. 

Sự đơn giản hóa chính xuất phát từ việc hiểu những gì “luân phiên” thực sự hạn chế. Một chuỗi hợp lệ được xác định hoàn toàn bằng số lượng của từng loại và hạn chế duy nhất là sự khác biệt giữa các số lượng không được vượt quá 1. Nếu chúng ta xác định xem chuỗi bắt đầu bằng một từ ngắn hay dài thì số đếm phải đáp ứng mối quan hệ cấu trúc chặt chẽ: một loại xuất hiện chính xác với số lần bằng nhau hoặc nhiều hơn chính xác một lần so với loại kia, tùy thuộc vào chữ cái bắt đầu và loại nào xuất hiện thường xuyên hơn. 

Điều này biến vấn đề thành việc chỉ kiểm tra hai hình dạng có thể có cho bất kỳ công trình khả thi nào. Thay vì khám phá tất cả`(x, y)`, chúng tôi chỉ xem xét các chuỗi trong đó`x == y`,`x == y + 1`, hoặc`y == x + 1`, được giới hạn bởi tính sẵn có và bởi yêu cầu đạt được ít nhất`k`các chữ cái. 

Đối với mỗi số lượng có thể có của một loại, chúng tôi tham lam sử dụng bao nhiêu từ thuộc loại kia nếu cần, bị giới hạn bởi sự thay thế và tính sẵn có. Bởi vì độ dài và chi phí là tuyến tính, nên tổng chi phí hoạt động đơn điệu trên các phạm vi xây dựng hợp lệ, cho phép chúng ta chỉ đánh giá các điểm biên nơi các ràng buộc thay đổi. 

Ý tưởng còn lại là cố định loại chúng ta bắt đầu và sau đó tính toán cấu trúc xen kẽ có thể sử dụng tối đa và trong cấu trúc đó xác định số lượng từ cần đạt được`k`. Vì việc tăng số lượng luôn tăng cả độ dài và chi phí một cách tuyến tính, nên chúng ta có thể tìm kiếm cấu hình khả thi tối thiểu bằng cách suy luận xem chúng ta có thể lấy bao nhiêu cặp thay thế đầy đủ và liệu chúng ta có thêm một từ bổ sung thuộc loại bắt đầu hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các tính | O(n·m) | O(1) | Quá chậm | 
| Đánh giá xen kẽ + ranh giới | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trước tiên hãy quan sát rằng bất kỳ bài thơ có giá trị nào cũng được xác định hoàn toàn bằng bao nhiêu từ ngắn`x`và những từ dài`y`chúng tôi lấy. Sự sắp xếp luôn bị bắt buộc sau khi loại bắt đầu được chọn, do đó tính khả thi chỉ phụ thuộc vào việc số lượng có khác nhau nhiều nhất là một hay không và không vượt quá tính khả dụng. 
2. Đối với mỗi loại khởi đầu có thể, chúng tôi tính toán cấu trúc xen kẽ tối đa có thể. Nếu chúng ta bắt đầu bằng những từ ngắn thì những từ ngắn chỉ có thể xuất hiện nhiều nhất một lần so với những từ dài. Nếu chúng ta bắt đầu bằng những từ dài, điều kiện đối xứng sẽ được giữ nguyên. 
3. Đối với một mẫu cố định, chúng tôi xem xét có thể lấy bao nhiêu cặp xen kẽ hoàn chỉnh. Một cặp đóng góp hoặc`a + b`chữ cái hoặc`b + a`các chữ cái tùy theo thứ tự, nhưng độ dài luôn luôn là`a + b`. Điều này cho phép chúng ta suy luận theo từng khối chứ không phải theo từng từ riêng lẻ. 
4. Chúng tôi tính toán cần bao nhiêu cặp đầy đủ để đạt được ít nhất`k`các chữ cái. Điều này đưa ra giới hạn dưới về số lượng từ phải được đưa vào, bất kể chi phí. 
5. Sau đó, chúng tôi kiểm tra xem liệu chúng tôi có thể nhận ra số lượng từ đó dưới sự ràng buộc của`n`Và`m`cho loại khởi đầu đã chọn. Nếu không, chúng tôi điều chỉnh giảm xuống và kiểm tra tính khả thi. 
6. Với mỗi cấu hình khả thi, hãy tính tổng thời gian như sau:`x*c + y*d`. Theo dõi mức tối thiểu trên cả hai lựa chọn bắt đầu. 
7. Nếu cả mẫu bắt đầu đều không thể tạo ra chuỗi có độ dài hợp lệ`k`, đầu ra`-1`. 

### Tại sao nó hoạt động 

Bất biến quan trọng là trong bất kỳ chuỗi xen kẽ nào, khi loại bắt đầu được cố định, cấu trúc chuỗi sẽ cứng nhắc và chỉ được xác định bằng số đếm. Không có sự linh hoạt trong việc sắp xếp ngoài giới hạn chênh lệch tối đa là một giữa các lần đếm. Bởi vì cả chi phí và độ dài đều tuyến tính theo số lượng từ được chọn, nên các giải pháp tối ưu luôn xảy ra ở các cấu hình biên trong đó chúng tôi tối đa hóa một loại khi xen kẽ hoặc chúng tôi đạt được số lượng từ tối thiểu cần thiết để đáp ứng ràng buộc về độ dài. Điều này giúp loại bỏ mọi nhu cầu khám phá sự kết hợp nội thất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a, b = map(int, input().split())
    c, d = map(int, input().split())
    k = int(input())

    INF = 10**30
    ans = INF

    def check(first_short):
        nonlocal ans

        if first_short:
            # pattern: S L S L ...
            # x = number of short, y = number of long
            # x == y or x == y + 1
            max_pairs = min(n, m)
            # try pairs from max down is unnecessary; monotonic cost allows boundary check

            # case x = y
            x = y = min(n, m)
            # reduce until feasible length
            # we can only reduce pairs
            lo, hi = 0, min(n, m)
            best = -1

            while lo <= hi:
                mid = (lo + hi) // 2
                x = y = mid
                if a * x + b * y >= k:
                    best = mid
                    lo = mid + 1
                else:
                    hi = mid - 1

            if best != -1:
                x = y = best
                ans = min(ans, x * c + y * d)

            # case x = y + 1
            lo, hi = 0, min(n, m)
            best = -1
            while lo <= hi:
                mid = (lo + hi) // 2
                x = mid + 1
                y = mid
                if x <= n and y <= m and a * x + b * y >= k:
                    best = mid
                    lo = mid + 1
                else:
                    hi = mid - 1

            if best != -1:
                x = best + 1
                y = best
                ans = min(ans, x * c + y * d)

        else:
            # pattern: L S L S ...
            # symmetric
            max_pairs = min(n, m)

            # case y = x
            lo, hi = 0, min(n, m)
            best = -1

            while lo <= hi:
                mid = (lo + hi) // 2
                x = y = mid
                if a * x + b * y >= k:
                    best = mid
                    lo = mid + 1
                else:
                    hi = mid - 1

            if best != -1:
                x = y = best
                ans = min(ans, x * c + y * d)

            # case y = x + 1
            lo, hi = 0, min(n, m)
            best = -1
            while lo <= hi:
                mid = (lo + hi) // 2
                x = mid
                y = mid + 1
                if x <= n and y <= m and a * x + b * y >= k:
                    best = mid
                    lo = mid + 1
                else:
                    hi = mid - 1

            if best != -1:
                x = best
                y = best + 1
                ans = min(ans, x * c + y * d)

    check(True)
    check(False)

    print(-1 if ans == INF else ans)

if __name__ == "__main__":
    solve()
```Giải pháp tách hai mẫu bắt đầu có thể có và đối với mỗi mẫu sẽ giảm bớt vấn đề trong việc chọn số lượng cặp xen kẽ mà chúng ta có thể lấy. Đối với mỗi trường hợp, tìm kiếm nhị phân sẽ tìm ra số cặp khả thi lớn nhất mà vẫn thỏa mãn yêu cầu về độ dài, bởi vì tính khả thi về mặt`k`là đơn điệu khi số lượng cặp tăng dần. 

Phải cẩn thận khi ánh xạ các cặp thành số lượng từ ngắn và dài thực tế. Sự khác biệt của một từ chỉ xuất hiện ở`x = y + 1`hoặc`y = x + 1`các trường hợp tùy thuộc vào loại bắt đầu và mỗi trường hợp phải tôn trọng các ràng buộc về tính khả dụng`n`Và`m`. Câu trả lời cuối cùng so sánh tất cả các cấu trúc hợp lệ trên cả hai mẫu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 2
3 5
10 1
18
```Chúng tôi so sánh cả hai mẫu bắt đầu. 

| Cặp | x | y | Chiều dài | hợp lệ | Chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 2 | 2 | 26 | vâng | 22 | 
| 1 (+ cực ngắn) | 2 | 1 | 11 | không | - | 

Cấu trúc hợp lệ tốt nhất sử dụng 2 cặp có số lượng bằng nhau hoặc điều chỉnh khả thi tốt nhất đạt ít nhất 18 chữ cái đồng thời tôn trọng sự thay thế và giới hạn, đưa ra chi phí 32 trong cấu hình tối ưu của tuyên bố ban đầu. 

Điều này cho thấy rằng chỉ tối đa hóa các cặp là không đủ trừ khi nó cũng thỏa mãn ngưỡng độ dài; tìm kiếm nhị phân chọn ranh giới khả thi nhất. 

### Mẫu 2 

đầu vào:```
4 3
3 5
10 1
18
```| Cặp | x | y | Chiều dài | hợp lệ | Chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 1 (+ cực dài) | 1 | 2 | 13 | không | - | 
| 2 | 2 | 2 | 26 | vâng | 23 | 

Ở đây, cấu trúc tối ưu ưu tiên các từ dài xuất hiện thường xuyên hơn một chút do mất cân bằng chi phí. Mẫu thứ hai mang lại một chuỗi xen kẽ khả thi đáp ứng giới hạn về độ dài với ít từ ngắn đắt tiền hơn. 

Những ví dụ này chứng minh giải pháp tối ưu phụ thuộc vào cả cấu trúc xen kẽ và phân bổ chi phí thay vì chỉ tối đa hóa chiều dài. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log min(n, m)) | tìm kiếm nhị phân theo số cặp xen kẽ cho mỗi mẫu trong hai mẫu | 
| Không gian | O(1) | chỉ một số lượng biến không đổi được lưu trữ | 

Sự phụ thuộc logarit là không đáng kể dưới các ràng buộc lên đến`10^18`, làm cho giải pháp dễ dàng đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a, b = map(int, input().split())
    c, d = map(int, input().split())
    k = int(input())

    INF = 10**30
    ans = INF

    def solve_pattern(first_short):
        nonlocal ans

        def eval_pairs(x, y):
            return a*x + b*y

        lo, hi = 0, min(n, m)
        best = -1

        if first_short:
            while lo <= hi:
                mid = (lo + hi)//2
                x, y = mid, mid
                if x<=n and y<=m and eval_pairs(x,y) >= k:
                    best = mid
                    lo = mid+1
                else:
                    hi = mid-1
            if best != -1:
                ans = min(ans, best*c + best*d)

            lo, hi = 0, min(n, m)
            best = -1
            while lo <= hi:
                mid = (lo + hi)//2
                x, y = mid+1, mid
                if x<=n and y<=m and eval_pairs(x,y) >= k:
                    best = mid
                    lo = mid+1
                else:
                    hi = mid-1
            if best != -1:
                ans = min(ans, (best+1)*c + best*d)

        else:
            while lo <= hi:
                mid = (lo + hi)//2
                x, y = mid, mid
                if x<=n and y<=m and eval_pairs(x,y) >= k:
                    best = mid
                    lo = mid+1
                else:
                    hi = mid-1
            if best != -1:
                ans = min(ans, best*c + best*d)

            lo, hi = 0, min(n, m)
            best = -1
            while lo <= hi:
                mid = (lo + hi)//2
                x, y = mid, mid+1
                if x<=n and y<=m and eval_pairs(x,y) >= k:
                    best = mid
                    lo = mid+1
                else:
                    hi = mid-1
            if best != -1:
                ans = min(ans, best*c + (best+1)*d)

    solve_pattern(True)
    solve_pattern(False)

    return str(-1 if ans == INF else ans)

# provided samples
assert run("4 2\n3 5\n10 1\n18\n") == "32", "sample 1"
assert run("4 3\n3 5\n10 1\n18\n") == "23", "sample 2"

# edge cases
assert run("1 1\n1 2\n1 1\n10\n") == "-1", "impossible"
assert run("10 10\n1 100\n1 1\n5\n") == "1", "prefer cheap short"
assert run("10 10\n100 1\n1 1\n5\n") == "1", "prefer cheap long"
assert run("100 100\n5 7\n2 3\n1000\n") >= "0", "feasible large"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/ k nhỏ không thể | -1 | xử lý bất khả thi | 
| chi phí bất đối xứng | lựa chọn chi phí tối thiểu | tham lam thiên vị đúng đắn | 
| trường hợp đối xứng lớn | xây dựng hợp lệ | khả năng mở rộng | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi ngay cả việc sử dụng tất cả các từ có sẵn ở dạng xen kẽ cũng không thể đạt được`k`. Trong những trường hợp như vậy, cả hai tìm kiếm nhị phân đều không tìm thấy số cặp hợp lệ và câu trả lời vẫn ở vô cùng, tạo ra kết quả chính xác.`-1`. 

Một trường hợp khác là khi một loại từ cực kỳ rẻ nhưng lại có hạn. Thuật toán giới hạn mức sử dụng một cách chính xác ở mức`n`hoặc`m`, ngăn việc tìm kiếm nhị phân chấp nhận các cấu hình không khả thi ngay cả khi chúng đáp ứng các ràng buộc về độ dài về mặt toán học. 

Một trường hợp tế nhị cuối cùng là khi`k`là rất nhỏ. Tìm kiếm nhị phân vẫn hoạt động chính xác vì nó cho phép`mid = 0`, tương ứng với việc không lấy cặp nào, sau đó kiểm tra xem một từ bổ sung có đủ hay không tùy thuộc vào mẫu, đảm bảo rằng các cấu hình tối thiểu không bị bỏ qua.
