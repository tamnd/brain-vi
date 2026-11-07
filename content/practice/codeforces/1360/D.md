---
problem: 1360D
contest_id: 1360
problem_index: D
name: "Buying Shovels"
contest_name: "Codeforces Round 644 (Div. 3)"
rating: 1300
tags: ["math", "number theory"]
answer: passed_samples
verified: true
solve_time_s: 307
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e3e41-0410-83ec-a5d4-0c1d78bc83af
---

#CF 1360D - Mua Xẻng 

**Đánh giá:** 1300 
**Tags:** toán, lý thuyết số 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 5 phút 7s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e3e41-0410-83ec-a5d4-0c1d78bc83af 

--- 

##Giải pháp 

## Hiểu vấn đề 

Polycarp muốn có được chính xác$n$xẻng, nhưng anh ta không thể trộn lẫn các kích cỡ gói khác nhau. Cửa hàng cung cấp các gói kích thước$1$bởi vì$k$, và sau khi anh ấy chọn kích thước gói hàng$i$, mọi giao dịch mua phải có cùng kích thước. Nhiệm vụ là chọn kích thước gói tốt nhất rồi mua càng ít gói càng tốt sao cho tổng số xẻng bằng đúng.$n$. 

Nói cách khác, với mỗi kích thước gói hợp lệ$i$, chúng tôi được phép đại diện$n$dưới dạng tổng của các số hạng giống hệt nhau$i + i + \dots + i$. Điều này chỉ có thể thực hiện được nếu$i$chia rẽ$n$, và trong trường hợp đó số lượng gói là$n / i$. Mục tiêu là giảm thiểu giá trị này trên tất cả$i \le k$sự chia rẽ đó$n$. 

Các ràng buộc cho phép lên đến$10^9$cho cả hai$n$Và$k$, với tối đa 100 trường hợp thử nghiệm. Điều này loại trừ mọi cách tiếp cận lặp lại trên tất cả các kích thước gói lên đến$k$cho mỗi trường hợp thử nghiệm, vì điều đó sẽ yêu cầu tới$10^11$hoạt động trong trường hợp xấu nhất. 

Một sai lầm ngây thơ đến từ việc thử tất cả$i$từ$1$ĐẾN$k$. Ví dụ, nếu$n = 10^9$Và$k = 10^9$, việc lặp lại tất cả các ứng cử viên là không thể trong giới hạn thời gian. 

Một trường hợp cạnh tinh tế khác xuất hiện khi$k \ge n$. Trong hoàn cảnh đó việc lựa chọn$i = n$được phép và đưa ra câu trả lời$1$. Một cách tiếp cận bất cẩn chỉ kiểm tra các ước của$n$nhưng bỏ qua ràng buộc giới hạn trên có thể bỏ lỡ phím tắt này. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi kích cỡ gói$i$từ$1$ĐẾN$k$, kiểm tra xem$i$chia rẽ$n$, và tính toán$n / i$. Câu trả lời là tối thiểu trong số tất cả các lựa chọn hợp lệ. Điều này đúng vì nó đánh giá rõ ràng mọi cấu hình được phép. 

Tuy nhiên, cách tiếp cận này thực hiện$O(k)$kiểm tra cho mỗi trường hợp thử nghiệm. Với$k$lên đến$10^9$, điều này trở nên không khả thi ngay cả đối với một trường hợp thử nghiệm duy nhất. 

Quan sát quan trọng là chúng ta không cần phải tìm kiếm tất cả các giá trị của$i$. Câu trả lời hay nhất tương ứng với việc làm$i$càng lớn càng tốt trong khi vẫn chia$n$, vì câu trả lời là$n / i$. Vì vậy, chúng ta muốn ước số lớn nhất của$n$điều đó không vượt quá$k$. 

Nếu như$k \ge n$, chúng ta có thể trực tiếp chọn$i = n$, đưa ra câu trả lời$1$. Ngược lại, chúng ta hạn chế ở các ước của$n$đó là nhiều nhất$k$. Ước số lớn nhất như vậy hoặc là ước số lớn được tìm thấy gần$\sqrt{n}$hoặc một số chia ghép với một số bù nhỏ. 

Chúng ta có thể lặp qua tất cả các ước của$n$lên đến$\sqrt{n}$, điều này hiệu quả vì bất kỳ cặp số chia nào$(d, n/d)$chứa một giá trị nhỏ và một giá trị lớn. Chúng tôi theo dõi ước số hợp lệ lớn nhất không vượt quá$k$, và tính toán$n$chia cho nó. 

Điều này làm giảm vấn đề từ việc quét tất cả các ứng viên cho đến$k$chỉ quét tối đa$\sqrt{n}$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(k)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Tối ưu |$O(\sqrt{n})$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu$k \ge n$, quay lại ngay$1$, bởi vì việc chọn kích thước gói$n$là hợp lệ và sử dụng một gói duy nhất. Đây là câu trả lời tối thiểu có thể có trên toàn cầu. 
2. Nếu không, hãy khởi tạo câu trả lời là$n$, tương ứng với lựa chọn hợp lý nhất$i = 1$. 
3. Lặp lại tất cả các số nguyên$d$từ$1$ĐẾN$\lfloor \sqrt{n} \rfloor$. Mỗi$d$đại diện cho một ứng cử viên chia tiềm năng. 
4. Nếu$d$chia rẽ$n$, xem xét cả hai$d$Và$n / d$như kích thước gói hợp lệ. 
5. Đối với mỗi ứng viên ước số hợp lệ$i$, kiểm tra xem$i \le k$. Nếu vậy, hãy cập nhật câu trả lời là$\min(\text{answer}, n / i)$. Bước này là hợp lý vì việc chọn kích thước gói$i$dẫn đến chính xác$n / i$gói. 
6. Sau khi kiểm tra tất cả các cặp ước số, hãy đưa ra câu trả lời đúng nhất tìm được. 

### Tại sao nó hoạt động 

Mọi nghiệm hợp lệ đều tương ứng với việc chọn một ước số$i$của$n$với$i \le k$. Thuật toán liệt kê tất cả các ước của$n$, do đó nó không bao giờ bỏ lỡ bất kỳ sự lựa chọn khả thi nào. Đối với mỗi ứng viên, nó tính toán chính xác số lượng gói hàng được yêu cầu. Vì mục tiêu là giảm thiểu$n / i$, tối đa hóa$i$dưới sự ràng buộc$i \mid n$Và$i \le k$, ứng cử viên tốt nhất được đảm bảo nằm trong số các ước số được liệt kê. Điều này đảm bảo tính chính xác mà không cần quét các giá trị không chia số hoặc giá trị ở trên$\sqrt{n}$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())

        if k >= n:
            print(1)
            continue

        ans = n

        r = int(math.isqrt(n))
        for d in range(1, r + 1):
            if n % d == 0:
                if d <= k:
                    ans = min(ans, n // d)
                other = n // d
                if other <= k:
                    ans = min(ans, n // other)

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xử lý trường hợp tối ưu tầm thường trong đó có thể có một gói duy nhất. Vòng chia số đảm bảo tất cả các kích thước gói ứng viên được kiểm tra mà không vượt quá$\sqrt{n}$sự phức tạp. Mỗi cặp ước số được xử lý cẩn thận, kiểm tra cả hai phần tử vì chỉ một phần tử có thể nằm trong giới hạn$k$. 

Một cạm bẫy triển khai phổ biến là quên kiểm tra cả hai$d$Và$n/d$, sẽ bỏ lỡ các ước số lớn hợp lệ. Một người khác không xử lý$k \ge n$phím tắt, nếu không thì vẫn hoạt động nhưng có thể lãng phí thời gian hoặc làm phức tạp logic một cách không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 8, k = 7$Chúng tôi kiểm tra các ước của 8. 

| d | d chia n | d ≤ k | không có | n/d ≤ k | câu trả lời hay nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | vâng | vâng | 8 | vâng | 1 | 
| 2 | vâng | vâng | 4 | vâng | 1 | 
| 3 | không | - | - | - | 1 | 
| 4 | vâng | vâng | 2 | vâng | 1 | 

Thuật toán nhận thấy rằng kích thước gói 4 mang lại$8 / 4 = 2$, là tối ưu trong số các tùy chọn hợp lệ. Bảng xác nhận rằng tồn tại nhiều ước số hợp lệ, nhưng số lượng gói nhỏ nhất đến từ ước số hợp lệ lớn nhất. 

### Ví dụ 2:$n = 6, k = 10$Từ$k \ge n$, chúng ta trả về ngay 1. 

| Bước | Tình trạng | Hành động | 
| --- | --- | --- | 
| Kiểm tra k ≥ n | 10 ≥ 6 | trở lại 1 | 

Điều này thể hiện trường hợp rút gọn trong đó kích thước gói tối ưu là toàn bộ$n$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \sqrt{n})$| mỗi bài kiểm tra kiểm tra các ước số lên đến$\sqrt{n}$| 
| Không gian |$O(1)$| chỉ sử dụng các biến không đổi | 

Với$t \le 100$Và$n \le 10^9$,$\sqrt{n}$là về$3 \times 10^4$, do đó tổng số hoạt động vẫn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    import math

    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())

        if k >= n:
            print(1)
            continue

        ans = n
        r = int(math.isqrt(n))
        for d in range(1, r + 1):
            if n % d == 0:
                if d <= k:
                    ans = min(ans, n // d)
                other = n // d
                if other <= k:
                    ans = min(ans, n // other)

        print(ans)

    return output.getvalue()

# provided samples
assert run("5\n8 7\n8 1\n6 10\n999999733 999999732\n999999733 999999733\n") == "2\n8\n1\n999999733\n1\n"

# custom cases
assert run("1\n1 1\n") == "1\n"
assert run("1\n10 3\n") == "5\n"
assert run("1\n12 12\n") == "1\n"
assert run("1\n100 7\n") == "4\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 1 | trường hợp biên nhỏ nhất | 
| 10 3 | 5 | lựa chọn ước số bị hạn chế | 
| 12 12 | 1 | k ≥ n phím tắt | 
| 100 7 | 4 | ứng viên nhiều ước số | 

## Vỏ cạnh 

Khi nào$n = 1$, kích thước gói duy nhất có thể là 1, vì vậy câu trả lời luôn là 1 bất kể$k$. Thuật toán xử lý ngay việc này vì$k \ge n$hoặc vì vòng chia số bao gồm$d = 1$, đưa ra câu trả lời$1 / 1 = 1$. 

Khi$k \ge n$, chẳng hạn như$n = 50, k = 100$, thuật toán kích hoạt việc quay trở lại sớm. Điều này tránh được việc tính toán số chia không cần thiết và trực tiếp tạo ra giải pháp gói đơn tối ưu. 

Khi$n$là số nguyên tố và$k < n$, chẳng hạn như$n = 999999733, k = 10$, ước số duy nhất không vượt quá$k$là 1. Vòng lặp không tìm thấy ước số hợp lệ lớn hơn, vì vậy câu trả lời vẫn là$n$, tương ứng với việc mua$n$gói kích thước 1.
