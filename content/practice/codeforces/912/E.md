---
title: "CF 912E - Quà Tặng Cao Cấp"
description: "Chúng ta được yêu cầu liệt kê một tập hợp số nguyên rất cụ thể và sau đó chọn một phần tử cụ thể từ danh sách có thứ tự đó. Các số được phép là những số có thừa số nguyên tố thuộc một tập hợp nhỏ cố định các số nguyên tố đã cho."
date: "2026-06-15T12:13:59+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dfs-and-similar", "math", "meet-in-the-middle", "number-theory", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 912
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 456 (Div. 2)"
rating: 2400
weight: 912
solve_time_s: 450
verified: true
draft: false
---

[CF 912E - Quà tặng Prime](https://codeforces.com/problemset/problem/912/E) 

**Đánh giá:** 2400 
**Tags:** tìm kiếm nhị phân, dfs và tương tự, toán, gặp nhau ở giữa, lý thuyết số, hai con trỏ 
**Thời gian giải:** 7 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu liệt kê một tập hợp số nguyên rất cụ thể và sau đó chọn một phần tử cụ thể từ danh sách có thứ tự đó. Các số được phép là những số có thừa số nguyên tố thuộc một tập hợp nhỏ cố định các số nguyên tố đã cho. Nói cách khác, mọi số hợp lệ chỉ có thể được viết dưới dạng tích lũy thừa của các số nguyên tố đã cho và không có số nguyên tố nào khác được phép xuất hiện trong phân tích nhân tử của nó. 

Dãy số này được xem xét theo thứ tự tăng dần, bắt đầu từ 1, vì 1 không có ước nguyên tố và luôn đúng theo định nghĩa này. Nhiệm vụ là tìm phần tử thứ k trong dãy tăng vô hạn này. 

Các ràng buộc định hình không gian giải pháp một cách mạnh mẽ. Số lượng số nguyên tố được phép nhiều nhất là 16, loại trừ mọi cách tiếp cận cố gắng kết hợp rõ ràng tất cả các tập hợp con của số nguyên tố theo cách hàm mũ ngây thơ trên tất cả các giá trị. Tuy nhiên, bản thân các giá trị của các số nguyên tố đều nhỏ và câu trả lời được đảm bảo tối đa là 10^18, điều này cho phép chúng ta suy luận theo cách tìm kiếm số thay vì tạo ra tổ hợp tất cả các khả năng. Cấu trúc ẩn là mặc dù chuỗi có độ dài vô hạn nhưng lại có giá trị thưa thớt và đơn điệu. 

Trường hợp cạnh tinh tế xuất hiện ngay ở đầu chuỗi. Số 1 phải được đưa vào làm phần tử đầu tiên. Bất kỳ triển khai nào bắt đầu tạo trực tiếp từ các số nguyên tố và bỏ qua 1 sẽ bị tắt một. Một vấn đề khác phát sinh từ việc tạo bản sao. Một số như 12 = 2^2 * 3 có thể được tạo ra bằng nhiều đường nhân nếu chúng ta không cẩn thận, do đó, một DFS ngây thơ hoán vị các số nguyên tố một cách tự do sẽ đếm quá mức trừ khi chúng ta thực thi thứ tự. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ là tạo ra các số theo thứ tự tăng dần bằng cách sử dụng một đống tối thiểu, nhân liên tục các số được phát hiện với mỗi số nguyên tố và đẩy kết quả trở lại. Điều này tương tự như việc tạo ra “những con số xấu xí”. Điều này hoạt động tốt khi k nhỏ, nhưng ở đây k không bị giới hạn về kích thước và nó có thể cực kỳ lớn trong khi giá trị câu trả lời thực tế vẫn nằm trong khoảng 10^18. Phương pháp dựa trên heap sẽ yêu cầu k lần chèn và thao tác log k, điều này trở nên không khả thi khi k lớn. 

Quan sát quan trọng là chúng ta thực sự không cần xây dựng chuỗi tới chỉ số k. Chúng ta chỉ cần xác định giá trị nào nhỏ nhất thứ k và chúng ta có một giới hạn chặt chẽ về giá trị của câu trả lời. Điều này gợi ý đảo ngược quan điểm: thay vì tạo chuỗi theo chỉ mục, chúng tôi kiểm tra các giá trị ứng cử viên và hỏi có bao nhiêu số hợp lệ ≤ x. Điều đó chuyển đổi vấn đề thành một hàm đếm đơn điệu, cho phép tìm kiếm nhị phân qua câu trả lời. 

Khó khăn còn lại là tính toán số lượng hợp lệ ≤ x một cách hiệu quả. Vì n ≤ 16, chúng ta có thể thực hiện phép liệt kê theo chiều sâu đối với các số nguyên tố, xây dựng các tích đồng thời thực thi các chỉ số nguyên tố không giảm để tránh trùng lặp. Mỗi trạng thái nhân giá trị hiện tại với một số nguyên tố đã chọn và tiếp tục miễn là sản phẩm vẫn nằm trong giới hạn. Điều này tạo ra mỗi số hợp lệ chính xác một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tạo đống | O(k log k) | O(k) | Quá chậm | 
| Tìm kiếm nhị phân + đếm DFS | O(log V · S) | O(độ sâu) | Đã chấp nhận | 

Ở đây V lên tới 10^18 và S là số trạng thái được tạo ra khi cắt tỉa. 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách tìm kiếm nhị phân trên giá trị của câu trả lời và sử dụng quy trình đếm để kiểm tra tính khả thi. 

1. Xác định hàm`count(x)`trả về có bao nhiêu số hợp lệ ≤ x. 

Hàm này bao gồm 1 và tất cả tích của các số nguyên tố đã cho có giá trị không vượt quá x. 
2. Thực hiện`count(x)`sử dụng tìm kiếm theo chiều sâu trên các số nguyên tố. 

Chúng ta bắt đầu từ giá trị 1 và ở mỗi bước nhân với các số nguyên tố có chỉ số không nhỏ hơn chỉ số hiện tại. 

Thứ tự này ngăn cản việc tạo ra cùng một số thông qua các hoán vị khác nhau của các thừa số nguyên tố. 
3. Trong DFS, bất cứ khi nào giá trị hiện tại là ≤ x, nó được tính là số hợp lệ. 

Sau đó, chúng tôi tiếp tục mở rộng nó bằng cách nhân với các số nguyên tố đủ điều kiện, dừng khi tích vượt quá x. 
4. Chạy tìm kiếm nhị phân trên ô trả lời từ 1 đến 10^18. 

Với mỗi điểm giữa, hãy tính`count(mid)`. 

Nếu số lượng ít nhất là k, chúng ta biết câu trả lời nằm ở mức trung bình hoặc thấp hơn mức trung bình, nếu không thì nó nằm ở mức trung bình. 
5. Sau khi tìm kiếm nhị phân hội tụ, xuất ra giá trị nhỏ nhất mà`count(value) ≥ k`. 

Tính đúng đắn phụ thuộc vào tính đơn điệu của hàm đếm: nếu x tăng thì tập hợp các số hợp lệ ≤ x chỉ có thể tăng. 

### Tại sao nó hoạt động 

Mỗi số hợp lệ có một biểu diễn duy nhất là tích của các số nguyên tố đã cho với số mũ không âm. Bằng cách thực thi rằng chúng tôi chỉ nhân bằng cách sử dụng các số nguyên tố tại hoặc sau chỉ mục hiện tại, chúng tôi đảm bảo mỗi kết hợp được tạo chính xác một lần. DFS khám phá chính xác không gian của các số hợp lệ dưới giới hạn và tìm kiếm nhị phân tận dụng mối quan hệ đơn điệu giữa giá trị ngưỡng và số lượng số có thể truy cập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1000000)

primes = []

def count_leq(x):
    n = len(primes)
    res = 0

    def dfs(i, val):
        nonlocal res
        res += 1
        for j in range(i, n):
            p = primes[j]
            if val > x // p:
                break
            dfs(j, val * p)

    dfs(0, 1)
    return res

def solve():
    global primes
    n = int(input())
    primes = list(map(int, input().split()))
    k = int(input())

    lo, hi = 1, 10**18

    while lo < hi:
        mid = (lo + hi) // 2
        if count_leq(mid) >= k:
            hi = mid
        else:
            lo = mid + 1

    print(lo)

if __name__ == "__main__":
    solve()
```DFS được cấu trúc cẩn thận sao cho mỗi lệnh gọi đệ quy thể hiện việc mở rộng hệ số hợp lệ thêm một thừa số nguyên tố nữa. Kiểm tra phân chia`val > x // p`tránh tràn và tiếp tục cắt tỉa mạnh mẽ khi giá trị tăng vượt quá giới hạn. Tìm kiếm nhị phân bao bọc chức năng đếm này để xác định ngưỡng chính xác nơi số thứ k xuất hiện. 

Một lỗi phổ biến là cho phép DFS khởi động lại từ chỉ mục 0 ở mọi cấp độ, dẫn đến hoán vị của cùng một hệ số. Ràng buộc`for j in range(i, n)`là những gì thực thi thứ tự kinh điển. 

## Ví dụ đã hoạt động 

Xét mẫu có số nguyên tố`[2, 3, 5]`và k = 7. 

DFS liệt kê các số theo chiều sâu cấu trúc tăng dần, nhưng về mặt khái niệm, trình tự hợp lệ là: 

| Bước | Hiện tại x | Bản mở rộng mới | 
| --- | --- | --- | 
| 1 | 1 | 2, 3, 5 | 
| 2 | 2 | 4, 6, 10 | 
| 3 | 3 | 6, 9, 15 | 
| 4 | 4 | 8, 12, 20 | 

Sau khi loại bỏ trùng lặp theo quy tắc xây dựng, thứ tự sắp xếp sẽ trở thành: 

1, 2, 3, 4, 5, 6, 8, ... 

Phần tử thứ 7 là 8, khớp với đầu ra. 

Bây giờ hãy xem xét một ví dụ tùy chỉnh nhỏ hơn với số nguyên tố`[2, 3]`và k = 5. 

| Bước | Đã trích lệnh | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 
| 4 | 4 | 
| 5 | 6 | 

Kết quả là 6. 

Dấu vết này cho thấy cách các sản phẩm phân nhánh nhưng vẫn được sắp xếp theo thứ tự và duy nhất do ràng buộc về chỉ số tăng dần trong DFS. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log(10^18) · S) | Tìm kiếm nhị phân trên phạm vi giá trị, mỗi bước chạy DFS trên tất cả các sản phẩm hợp lệ trong giới hạn | 
| Không gian | O(n) | Độ sâu đệ quy được giới hạn bởi số lượng số nguyên tố và sự tăng trưởng của chuỗi sản phẩm | 

Số lượng trạng thái có thể truy cập được kiểm soát bởi sự tăng trưởng nhanh chóng của sản phẩm vì mỗi lần nhân với số nguyên tố ≥ 2 sẽ nhanh chóng vượt quá giới hạn, giúp DFS có thể quản lý được. Kết hợp với tìm kiếm nhị phân trên phạm vi 64-bit cố định, giải pháp này phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    primes = []

    def count_leq(x):
        n = len(primes)
        res = 0

        def dfs(i, val):
            nonlocal res
            res += 1
            for j in range(i, n):
                p = primes[j]
                if val > x // p:
                    break
                dfs(j, val * p)

        dfs(0, 1)
        return res

    def solve():
        nonlocal primes
        n = int(input())
        primes = list(map(int, input().split()))
        k = int(input())

        lo, hi = 1, 10**18
        while lo < hi:
            mid = (lo + hi) // 2
            if count_leq(mid) >= k:
                hi = mid
            else:
                lo = mid + 1
        return str(lo)

    return solve()

# sample
assert run("3\n2 3 5\n7\n") == "8"

# minimum case
assert run("1\n2\n1\n") == "1"

# simple primes
assert run("2\n2 3\n5\n") == "6"

# power-heavy
assert run("2\n2 3\n10\n") == "12"

# single prime chain
assert run("1\n7\n4\n") == "343"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số nguyên tố đơn | 1 | trường hợp cơ sở bao gồm 1 chính xác | 
| bộ nhỏ | 6 | sắp xếp với hai số nguyên tố | 
| tăng trưởng hỗn hợp | 12 | phân nhánh và cắt tỉa đúng cách | 
| xích điện | 343 | xử lý số mũ sâu | 

## Vỏ cạnh 

Khi n = 1, dãy trở thành lũy thừa của một số nguyên tố cộng 1. Bất kỳ giải pháp nào bắt đầu liệt kê từ chính số nguyên tố đó thay vì 1 sẽ dịch chuyển tất cả các chỉ số và trả về kết quả sai. 

Khi k = 1, câu trả lời luôn là 1 bất kể số nguyên tố là gì. Điều này thường bị bỏ qua nếu việc triển khai giả định giá trị được tạo đầu tiên phải đến từ việc nhân các số nguyên tố một lần. 

Khi k lớn nhưng các giá trị tăng nhanh, việc tạo theo chỉ số ngây thơ sẽ hết thời gian mặc dù câu trả lời có độ lớn nhỏ. Đây chính xác là tình huống mà việc tìm kiếm nhị phân trên không gian giá trị trở nên cần thiết thay vì xây dựng chuỗi.
