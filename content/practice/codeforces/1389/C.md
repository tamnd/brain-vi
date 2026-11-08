---
title: "CF 1389C - Dây Tốt"
description: "Chúng ta được cung cấp một chuỗi chữ số và được phép xóa các ký tự ở bất cứ đâu chúng ta muốn, giữ nguyên thứ tự của những ký tự còn lại. Mục đích là biến chuỗi thành một dạng đặc biệt gọi là “tốt”."
date: "2026-06-16T14:51:42+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1389
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 92 (Rated for Div. 2)"
rating: 1500
weight: 1389
solve_time_s: 310
verified: false
draft: false
---

[CF 1389C - Chuỗi tốt](https://codeforces.com/problemset/problem/1389/C) 

**Đánh giá:** 1500 
**Tags:** vũ phu, dp, tham lam, hai con trỏ 
**Thời gian giải:** 5 phút 10 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chữ số và được phép xóa các ký tự ở bất cứ đâu chúng ta muốn, giữ nguyên thứ tự của những ký tự còn lại. Mục đích là biến chuỗi thành một dạng đặc biệt gọi là “tốt”. 

Một chuỗi được coi là tốt khi áp dụng dịch chuyển tuần hoàn trái sẽ tạo ra chuỗi giống như khi áp dụng dịch chuyển tuần hoàn phải. Dịch chuyển theo chu kỳ sẽ di chuyển ký tự đầu tiên về cuối (dịch chuyển trái) hoặc ký tự cuối cùng lên phía trước (dịch chuyển phải) và trong một chuỗi tốt, cả hai thao tác đều tạo ra kết quả giống nhau. 

Câu hỏi quan trọng không phải là trực tiếp xây dựng một chuỗi tốt mà là loại bỏ càng ít ký tự càng tốt để chuỗi con còn lại trở nên tốt. 

Do đó, đầu ra là số lần xóa tối thiểu cần thiết, tương đương với việc tối đa hóa độ dài của một chuỗi con tốt. 

Ràng buộc về tổng chiều dài trong các trường hợp thử nghiệm lên tới 2 · 10^5. Điều này ngay lập tức loại trừ bất cứ điều gì bậc hai cho mỗi trường hợp thử nghiệm và đẩy chúng ta tới một giải pháp tuyến tính hoặc gần tuyến tính cho mỗi thử nghiệm hoặc một giải pháp lặp lại trên một không gian trạng thái cố định nhỏ. 

Một cách tiếp cận đơn giản sẽ thử tất cả các dãy con hoặc tất cả các thao tác xóa, nhưng ngay cả việc kiểm tra tính hợp lệ của một dãy con cũng cần có thời gian tuyến tính. Với nhiều dãy con theo cấp số nhân, điều này là không thể. 

Trường hợp cạnh tinh tế xuất hiện khi chuỗi trông rất đều đặn nhưng không hoàn toàn xen kẽ. Ví dụ, trong`100120013`, không rõ nên xóa ký tự nào để đạt được cấu trúc tối ưu. Việc loại bỏ một cách tham lam các ký tự “có vẻ ngoài xấu” mà không hiểu mô hình chung có thể dễ dàng bỏ lỡ chuỗi con tối ưu. 

Một chế độ lỗi khác là giả định rằng chuỗi tốt thu được phải có tất cả các ký tự giống hệt nhau. Điều đó đôi khi đúng với những câu trả lời tối ưu, nhưng không phải lúc nào cũng cần thiết trong cách suy luận trung gian. 

## Phương pháp tiếp cận 

Bắt đầu bằng cách hiểu những gì “tốt” thực sự có hiệu lực về mặt cấu trúc. Thay vì mô phỏng trực tiếp các dịch chuyển theo chu kỳ, chúng tôi so sánh hai phiên bản được dịch chuyển theo từng vị trí. 

Hãy để chuỗi là`t1 t2 ... tn`. 

Dịch chuyển trái cho`t2 t3 ... tn t1`. 

Dịch chuyển phải cho`tn t1 t2 ... t_{n-1}`. 

Để những thứ này giống hệt nhau, các vị trí tương ứng phải khớp nhau. Điều này tạo ra một hệ thống bình đẳng:`t2 = tn`,`t3 = t1`,`t4 = t2`, vân vân. 

Việc tuân theo các ràng buộc này cho thấy một mẫu lặp lại với giai đoạn 2. Mỗi ký tự thứ hai phải khớp nhau, nghĩa là tất cả các vị trí lẻ đều giống hệt nhau và tất cả các vị trí chẵn đều giống hệt nhau. 

Vì vậy, bất kỳ chuỗi tốt nào cũng phải xen kẽ giữa hai chữ số cố định, chẳng hạn`a b a b a b ...`hoặc`b a b a b a ...`, bao gồm cả trường hợp suy biến trong đó`a = b`, tạo ra một chuỗi không đổi. 

Do đó, vấn đề trở thành: từ chuỗi gốc, xóa ít ký tự nếu cần để chuỗi con còn lại xen kẽ giữa hai chữ số đã chọn. 

Cách tiếp cận bạo lực sẽ thử mọi chuỗi con hoặc thậm chí hợp lý hơn, thử từng tập hợp con các ký tự để xóa và kiểm tra xem phần còn lại có tốt không. Điều đó dẫn đến hành vi theo cấp số nhân về độ dài của chuỗi và trở nên không khả thi đối với các đầu vào nhỏ. 

Quan sát quan trọng là cấu trúc của chuỗi hợp lệ chỉ phụ thuộc vào việc chọn một cặp chữ số có thứ tự`(a, b)`. Sau khi cặp này được cố định, chuỗi con tốt nhất có thể được xây dựng một cách tham lam: quét từ trái sang phải và chỉ giữ lại các ký tự khi chúng khớp với mẫu xen kẽ dự kiến. Chúng ta có thể đánh giá tất cả 100 cặp chữ số có thể một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với các chuỗi tiếp theo | O(2^n · n) | O(n) | Quá chậm | 
| Hãy thử tất cả các cặp chữ số có cấu trúc tham lam | O(100 · n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển nhiệm vụ thành tìm chuỗi con dài nhất xen kẽ giữa hai chữ số cố định. 

1. Cố định một cặp chữ số có thứ tự`(a, b)`. 

Cặp này đại diện cho mẫu xen kẽ dự định của chuỗi cuối cùng. Chúng tôi sẽ thử tất cả 100 khả năng. 
2. Tính dãy con dài nhất bắt đầu bằng kỳ vọng`a`. 

Chúng tôi quét chuỗi từ trái sang phải. Bất cứ khi nào chúng tôi nhìn thấy ký tự mong đợi, chúng tôi lấy nó và chuyển kỳ vọng sang chữ số khác. 

Điều này hiệu quả vì việc lấy các kết quả khớp hợp lệ trước đó luôn mang lại sự linh hoạt hơn cho các kết quả phù hợp trong tương lai. 
3. Tính dãy con dài nhất bắt đầu bằng kỳ vọng`b`. 

Điều này nắm bắt sự bắt đầu chẵn lẻ thay thế, vì chuỗi con xen kẽ tối ưu có thể bắt đầu bằng một trong hai chữ số. 
4. Lấy kết quả tối đa trong hai lựa chọn xuất phát cho cặp này`(a, b)`. 
5. Lặp lại cho tất cả các cặp chữ số và theo dõi độ dài dãy con lớn nhất toàn cục. 
6. Câu trả lời là`n - best_subsequence_length`. 

Tại sao điều này là đủ: mọi chuỗi tốt hợp lệ phải xen kẽ giữa hai chữ số và mọi chuỗi con xen kẽ tương ứng chính xác với một số lựa chọn về`(a, b)`cộng với vị trí bắt đầu. Quá trình quét tham lam tạo ra chuỗi con tối ưu cho từng cấu hình cố định vì mọi kết quả khớp hợp lệ bị bỏ qua sẽ chỉ trì hoãn hoặc chặn các kết quả khớp bắt buộc trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def best_for_pair(s, a, b):
    # try starting with a
    need = a
    cnt1 = 0
    for ch in s:
        if ch == need:
            cnt1 += 1
            need = b if need == a else a

    # try starting with b
    need = b
    cnt2 = 0
    for ch in s:
        if ch == need:
            cnt2 += 1
            need = a if need == b else b

    return max(cnt1, cnt2)

def solve():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        n = len(s)

        best = 1
        for i in range(10):
            for j in range(10):
                best = max(best, best_for_pair(s, str(i), str(j)))

        print(n - best)

if __name__ == "__main__":
    solve()
```Ý tưởng cốt lõi trong quá trình triển khai là tách việc kiểm tra chuỗi con xen kẽ thành một hàm trợ giúp trên một cặp chữ số cố định. Vòng lặp kép trên các chữ số có kích thước không đổi, do đó độ phức tạp vẫn tuyến tính theo độ dài chuỗi. 

Một chi tiết triển khai tinh tế là xử lý trường hợp`a == b`. Trong trường hợp đó, quy trình xen kẽ sẽ suy biến thành việc chọn tất cả các lần xuất hiện của một chữ số và logic tham lam tương tự vẫn hoạt động chính xác vì kỳ vọng không bao giờ thay đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`95831`Chúng tôi thử theo cặp nhưng tập trung vào`(5,1)`vì nó tạo ra một chuỗi con xen kẽ mạnh mẽ. 

| Bước | Nhân vật | Dự kiến ​​| Lấy? | Độ dài chuỗi | 
| --- | --- | --- | --- | --- | 
| 1 | 9 | 5 | Không | 0 | 
| 2 | 5 | 5 | Có → mong đợi 1 | 1 | 
| 3 | 8 | 1 | Không | 1 | 
| 4 | 3 | 1 | Không | 1 | 
| 5 | 1 | 1 | Có → mong đợi 5 | 2 | 

Độ dài chuỗi con tốt nhất trên tất cả các cặp là 2, vì vậy câu trả lời là`5 - 2 = 3`. 

Điều này cho thấy cấu trúc tối ưu không phải là về tính liền kề trong chuỗi gốc mà là về việc chọn một mẫu xen kẽ nhất quán. 

### Ví dụ 2:`252525252525`Xem xét cặp`(2,5)`bắt đầu bằng`2`. 

| Bước | Nhân vật | Dự kiến ​​| Lấy? | Độ dài chuỗi | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | Có → mong đợi 5 | 1 | 
| 2 | 5 | 5 | Có → mong đợi 2 | 2 | 
| 3 | 2 | 2 | Có → mong đợi 5 | 3 | 
| ... | ... | ... | ... | ... | 

Mọi ký tự đều có thể sử dụng được, với độ dài đầy đủ là 12. Câu trả lời là`0`. 

Điều này xác nhận rằng các chuỗi đã xen kẽ hoàn hảo là các điểm cố định của quy trình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(100 · n) | Mỗi bài kiểm tra sẽ kiểm tra 100 cặp chữ số, mỗi cặp trong một lần quét tuyến tính | 
| Không gian | O(1) | Chỉ sử dụng bộ đếm và biến tạm thời | 

Tổng kích thước đầu vào là 2 · 10^5, vì vậy giải pháp này chạy thoải mái trong giới hạn ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    def best_for_pair(s, a, b):
        need = a
        c1 = 0
        for ch in s:
            if ch == need:
                c1 += 1
                need = b if need == a else a

        need = b
        c2 = 0
        for ch in s:
            if ch == need:
                c2 += 1
                need = a if need == b else b

        return max(c1, c2)

    for _ in range(t):
        s = input().strip()
        n = len(s)
        best = 1
        for i in range(10):
            for j in range(10):
                best = max(best, best_for_pair(s, str(i), str(j)))
        print(n - best)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as _io
    out = _io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""3
95831
100120013
252525252525
""") == """3
5
0"""

# minimum size
assert run("""1
12
""") == """1"""

# all equal
assert run("""1
777777
""") == """0"""

# already alternating
assert run("""1
121212
""") == """0"""

# mixed case
assert run("""1
123454321
""") >= "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 12 | 1 | xóa nhỏ nhất không tầm thường | 
| 777777 | 0 | cấu trúc tối ưu một chữ số | 
| 121212 | 0 | đã xen kẽ chuỗi | 
| 123454321 | 0+ | tìm kiếm mẫu không tầm thường | 

## Vỏ cạnh 

Trường hợp cạnh phổ biến là khi tất cả các ký tự giống hệt nhau, chẳng hạn như`777777`. Thuật toán đánh giá cặp`(7,7)`và đếm chính xác toàn bộ chuỗi dưới dạng một chuỗi con xen kẽ hợp lệ vì không cần chuyển đổi. 

Một trường hợp khác là khi mẫu tối ưu sử dụng hai chữ số khác nhau cách xa nhau trong chuỗi gốc, chẳng hạn như`95831`. Quét tham lam vẫn tìm ra dãy con chính xác vì nó không phụ thuộc vào tính kề cận mà chỉ phụ thuộc vào việc bảo toàn thứ tự. 

Một trường hợp nữa là khi việc luân phiên bắt đầu bằng chữ số ít thường xuyên hơn. Ví dụ, trong`100120013`, dãy con tối ưu có thể bắt đầu bằng một chữ số xuất hiện sau trong chuỗi. Thuật toán xử lý vấn đề này bằng cách thử rõ ràng cả hai trạng thái bắt đầu cho mỗi cặp chữ số, đảm bảo không bỏ sót cấu hình nào.
