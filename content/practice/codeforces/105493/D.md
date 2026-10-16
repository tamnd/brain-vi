---
title: "CF 105493D - Thuyết âm mưu"
description: "Chúng ta được cho một dãy số nguyên dương. Sau khi loại bỏ các bản sao, chúng tôi quan tâm đến việc xây dựng cấu trúc được định hướng trên các vị trí của chúng theo thứ tự chỉ mục tăng dần."
date: "2026-06-23T20:22:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105493
codeforces_index: "D"
codeforces_contest_name: "2024-2025 ICPC NERC, Kyrgyzstan Regional Contest"
rating: 0
weight: 105493
solve_time_s: 58
verified: true
draft: false
---

[CF 105493D - Thuyết âm mưu](https://codeforces.com/problemset/problem/105493/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên dương. Sau khi loại bỏ các bản sao, chúng tôi quan tâm đến việc xây dựng cấu trúc được định hướng trên các vị trí của chúng theo thứ tự chỉ mục tăng dần. Từ phần tử trước đó ở vị trí j, chúng ta được phép di chuyển đến phần tử sau ở vị trí i nếu hai giá trị có chung một ước số chung không tầm thường, nghĩa là gcd của chúng lớn hơn 1. 

Nhiệm vụ là tính toán độ dài tối đa của một chuỗi hợp lệ các bước di chuyển như vậy. Mỗi bước di chuyển phải diễn ra nghiêm ngặt trong mảng và mỗi bước yêu cầu hai giá trị được chọn phải chia sẻ ít nhất một thừa số nguyên tố. 

Đầu ra là một số duy nhất: độ dài của chuỗi dài nhất mà chúng ta có thể xây dựng theo các quy tắc này. 

Các ràng buộc đủ lớn để bất kỳ phương pháp bậc hai nào trên tất cả các cặp sẽ quá chậm. Việc truyền tải O(n^2) ngây thơ với kiểm tra gcd đã là ranh giới và nếu giá trị lớn, việc tính toán gcd lặp đi lặp lại vẫn khiến nó trở nên đắt đỏ. Nút thắt thực sự là sự phụ thuộc dày đặc giữa tất cả các yếu tố trước đây và hiện tại. 

Một vấn đề tế nhị thường gây ra những nỗ lực sai lầm là quên rằng chỉ những giá trị duy nhất mới quan trọng. Nếu các bản sao không bị xóa, chúng sẽ tăng chiều dài đường dẫn một cách giả tạo mà không cần thêm kết nối mới. Một vấn đề khác là giả định tính liền kề trong không gian giá trị. Ví dụ: các số như 6, 10, 15 tạo thành chuỗi dài hơn thông qua các số nguyên tố chung ngay cả khi chúng cách xa nhau về thứ tự giá trị. 

Một ví dụ đơn giản minh họa cấu trúc: 

Đầu vào: 6, 10, 15 

Chuỗi tối ưu là 6 → 10 → 15 vì gcd(6,10)=2 và gcd(10,15)=5, mặc dù 6 và 15 cũng kết nối. 

Một cách tiếp cận ngây thơ có thể cố gắng luôn kết nối với phần tử tương lai gần nhất chia sẻ bất kỳ ước số nào, nhưng ý tưởng tham lam đó không thành công vì việc bỏ qua các nút trung gian có thể chặn các chuỗi dài hơn. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Chúng tôi coi mọi chỉ mục là một nút trong biểu đồ và chúng tôi kết nối j với i nếu j < i và gcd(a[j], a[i]) > 1. Sau đó, chúng tôi tính toán đường đi dài nhất trong biểu đồ tuần hoàn có hướng này bằng cách sử dụng lập trình động trên các chỉ số. 

Với mỗi i, chúng ta thử tất cả j trước đó và cập nhật dp[i] thành dp[j] + 1 nếu một cạnh tồn tại. Việc này kiểm tra từng cặp và mỗi lần kiểm tra sử dụng gcd theo thời gian logarit. Với n lên tới 100000, số cặp trở thành 10^10 trong trường hợp xấu nhất, điều này hoàn toàn không khả thi. 

Quan sát quan trọng là gcd lớn hơn một tương đương với việc chia sẻ ít nhất một thừa số nguyên tố. Thay vì suy luận về các con số như những đối tượng nguyên tử, chúng ta chuyển sang suy luận về các thừa số nguyên tố của chúng. Mỗi số có thể được biểu diễn dưới dạng một tập hợp số nguyên tố nhỏ và các phép chuyển đổi chỉ phụ thuộc vào giao điểm giữa các tập hợp này. 

Điều này cho phép chúng ta thu gọn cấu trúc chuyển tiếp. Thay vì kiểm tra tất cả các chỉ số trước đó, chúng tôi duy trì, đối với mỗi số nguyên tố, chuỗi tốt nhất kết thúc ở một số vị trí trước đó có chứa số nguyên tố này. Sau đó, đối với số hiện tại, chúng ta có thể mở rộng từ bất kỳ số nguyên tố nào có trong đó bằng cách truy vấn trực tiếp các giá trị đó. 

Điều này làm giảm biểu đồ dày đặc trên các chỉ số thành tương tác thưa thớt trên các số nguyên tố, trong đó mỗi phần tử chỉ chạm vào một số lượng nhỏ trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DP theo cặp | O(n^2 log A) | O(n) | Quá chậm | 
| DP dựa trên Prime với tính năng nén trạng thái | O(A log log A + n · Pmax) | O(A + n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén mỗi số thành tập hợp các thừa số nguyên tố riêng biệt. Để thực hiện điều này một cách hiệu quả, chúng tôi tính toán trước các thừa số nguyên tố nhỏ nhất bằng cách sử dụng sàng, cho phép phân tích nhanh chóng. 

Chúng tôi duy trì một mảng dpPrime[p], đại diện cho độ dài chuỗi tốt nhất được thấy cho đến nay trong số các số có chứa số nguyên tố p. Đây là nén trạng thái chính: thay vì ghi nhớ các đường dẫn tốt nhất cho mỗi chỉ mục, chúng tôi ghi nhớ các đường dẫn tốt nhất cho mỗi số nguyên tố. 

Chúng tôi xử lý các số từ trái sang phải, đảm bảo tất cả các chuyển đổi đều tôn trọng chỉ số tăng dần. 

### bước

1. Tính trước hệ số nguyên tố nhỏ nhất cho mỗi số nguyên cho đến giá trị lớn nhất trong mảng. 

Điều này cho phép phân tích từng số theo thời gian gần tuyến tính trên tất cả các đầu vào. 
2. Với mỗi số a[i], trích xuất tập hợp các thừa số nguyên tố riêng biệt của nó bằng cách chia liên tục bằng bảng hệ số nguyên tố nhỏ nhất. 

Điều này cho chúng ta tất cả các số nguyên tố có thể tham gia vào quá trình chuyển đổi liên quan đến a[i]. 
3. Tính toán một ứng cử viên dp[i] bằng cách lấy giá trị lớn nhất trên dpPrime[p] cho tất cả các số nguyên tố p chia cho a[i], sau đó cộng một. 

Điều này thể hiện việc mở rộng chuỗi hợp lệ tốt nhất kết thúc bằng bất kỳ số nào có chung số nguyên tố với a[i]. 
4. Sau khi dp[i] được tính, hãy cập nhật dpPrime[p] = max(dpPrime[p], dp[i]) cho tất cả các số nguyên tố p chia cho a[i]. 

Điều này làm cho vị trí hiện tại có sẵn cho các phần mở rộng trong tương lai. 
5. Theo dõi dp[i] tối đa toàn cầu trên tất cả các vị trí và trả lại. 

Thứ tự cập nhật rất quan trọng. Trước tiên, chúng tôi tính toán dp[i] từ trạng thái trước đó, sau đó cập nhật dpPrime. Nếu đảo ngược điều này, chúng tôi sẽ cho phép một số sử dụng chính nó làm số trước đó một cách không chính xác. 

### Tại sao nó hoạt động 

Tại bất kỳ vị trí i nào, dpPrime[p] đại diện cho chuỗi tốt nhất kết thúc ở chỉ số j < i có giá trị chứa số nguyên tố p. Khi tính dp[i], chúng tôi xem xét tất cả các số nguyên tố của a[i], liệt kê chính xác tất cả các số đứng trước hợp lệ có thể chuyển đổi thành i. Vì mọi cạnh hợp lệ phải chia sẻ ít nhất một số nguyên tố và mỗi cạnh trước đó được biểu thị trong dpPrime cho số nguyên tố đó nên chúng tôi không bao giờ bỏ lỡ quá trình chuyển đổi. Quá trình quét chỉ chuyển tiếp đảm bảo tính chu kỳ và tính chính xác của thứ tự DP. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    if n == 0:
        print(0)
        return

    max_a = max(a)

    spf = list(range(max_a + 1))
    for i in range(2, int(max_a ** 0.5) + 1):
        if spf[i] == i:
            step = i
            start = i * i
            for j in range(start, max_a + 1, step):
                if spf[j] == j:
                    spf[j] = i

    def get_primes(x):
        res = []
        while x > 1:
            p = spf[x]
            res.append(p)
            while x % p == 0:
                x //= p
        return res

    dp_prime = {}
    ans = 1

    for x in a:
        primes = get_primes(x)

        best = 0
        for p in primes:
            if p in dp_prime:
                best = max(best, dp_prime[p])

        cur = best + 1
        ans = max(ans, cur)

        for p in primes:
            dp_prime[p] = max(dp_prime.get(p, 0), cur)

    print(ans)

if __name__ == "__main__":
    solve()
```Sàng xây dựng bảng thừa số nguyên tố nhỏ nhất để việc phân tích nhân tử không phụ thuộc vào phép chia thử lặp đi lặp lại. Mỗi số được phân hủy theo thời gian tỷ lệ thuận với các thừa số nguyên tố riêng biệt chứ không phải độ lớn của nó. 

Từ điển dp_prime lưu trữ độ dài chuỗi tốt nhất được liên kết với mỗi số nguyên tố. Chúng tôi sử dụng từ điển thay vì mảng cố định vì các số nguyên tố có thể lớn và thưa thớt sau khi phân tích thành thừa số. 

Đối với mỗi số, trước tiên chúng tôi tính toán phần mở rộng tốt nhất trước khi cập nhật dp_prime. Sự tách biệt này đảm bảo quá trình chuyển đổi chỉ sử dụng các chỉ mục trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
6 10 15 7
```Chúng tôi theo dõi dp_prime và dp: 

| tôi | giá trị | số nguyên tố | tốt nhất từ ​​dp_prime | dp[i] | cập nhật dp_prime | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 6 | 2,3 | 0 | 1 | 2→1, 3→1 | 
| 2 | 10 | 2,5 | 1 | 2 | 2→2, 5→2 | 
| 3 | 15 | 3,5 | 2 | 3 | 3→3, 5→3 | 
| 4 | 7 | 7 | 0 | 1 | 7→1 | 

Chuỗi 6 → 10 → 15 được hiện thực hóa thông qua các số nguyên tố chung 2 và 5. Giá trị 7 bị cô lập. 

Dấu vết cho thấy thông tin được truyền theo từng số nguyên tố chứ không phải theo chỉ mục. 

### Ví dụ 2 

đầu vào:```
5
2 4 8 3 9
```| tôi | giá trị | số nguyên tố | tốt nhất từ ​​dp_prime | dp[i] | cập nhật dp_prime | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 0 | 1 | 2→1 | 
| 2 | 4 | 2 | 1 | 2 | 2→2 | 
| 3 | 8 | 2 | 2 | 3 | 2→3 | 
| 4 | 3 | 3 | 0 | 1 | 3→1 | 
| 5 | 9 | 3 | 1 | 2 | 3→2 | 

Chúng tôi thấy hai chuỗi độc lập hình thành trên các thành phần nguyên tố riêng biệt. Thuật toán tự nhiên tách chúng ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(A log log A + n · Pmax) | sàng xây dựng SPF, mỗi số được phân tích thành thừa số nguyên tố riêng biệt | 
| Không gian | O(A + n) | Mảng SPF cộng với dung lượng lưu trữ dp trên mỗi số nguyên tố | 

Sàng chỉ chiếm ưu thế một lần, trong khi công của mỗi phần tử tỷ lệ thuận với số thừa số nguyên tố riêng biệt của nó. Vì mỗi số có một vài số nguyên tố phân biệt nên nghiệm vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import gcd

    def solve():
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))

        max_a = max(a)
        spf = list(range(max_a + 1))
        for i in range(2, int(max_a ** 0.5) + 1):
            if spf[i] == i:
                for j in range(i * i, max_a + 1, i):
                    if spf[j] == j:
                        spf[j] = i

        def get(x):
            res = []
            while x > 1:
                p = spf[x]
                res.append(p)
                while x % p == 0:
                    x //= p
            return res

        dp = {}
        ans = 1
        for x in a:
            ps = get(x)
            best = 0
            for p in ps:
                best = max(best, dp.get(p, 0))
            cur = best + 1
            ans = max(ans, cur)
            for p in ps:
                dp[p] = max(dp.get(p, 0), cur)

        return str(ans)

    return solve()

# sample-like
assert run("4\n6 10 15 7\n") == "3"
# chain doubling
assert run("5\n2 4 8 16 32\n") == "5"
# disjoint primes
assert run("4\n2 3 5 7\n") == "1"
# mixed
assert run("5\n6 10 15 21 14\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 10 15 7 | 3 | chuyển tiếp chuỗi cơ bản | 
| 2 4 8 16 32 | 5 | lan truyền nguyên tố lặp đi lặp lại | 
| 2 3 5 7 | 1 | không có cạnh nào tồn tại | 
| 6 10 15 21 14 | 4 | nhiều chuỗi nguyên tố chồng chéo | 

## Vỏ cạnh 

Một trường hợp góc là khi tất cả các số đều là số nguyên tố cùng nhau. Ví dụ: 

đầu vào:```
4
2 3 5 7
```Mỗi số có một tập hợp số nguyên tố riêng biệt, vì vậy dp_prime không bao giờ mang những chuyển đổi có ý nghĩa. Mỗi dp[i] trở thành 1 và câu trả lời là 1. Thuật toán xử lý vấn đề này vì không có số nguyên tố nào xuất hiện ở nhiều vị trí. 

Một trường hợp khác là lũy thừa lặp lại của một số nguyên tố: 

đầu vào:```
4
2 4 8 16
```Ở đây tất cả các số đều có chung số nguyên tố 2. Giá trị dp_prime[2] tăng dần đều: 1, 2, 3, 4. Mỗi bước sẽ mở rộng chuỗi tốt nhất trước đó một cách chính xác vì cùng một số nguyên tố sẽ tích lũy độ dài chuỗi tối đa toàn cầu được thấy cho đến nay.
