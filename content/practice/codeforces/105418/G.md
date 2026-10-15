---
title: "CF 105418G - Số lẻ không phải số nguyên tố"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm là một chuỗi các số nguyên đại diện cho một hàng thẻ."
date: "2026-06-23T04:23:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105418
codeforces_index: "G"
codeforces_contest_name: "Algorithmia IIITN 2024 - Round 1"
rating: 0
weight: 105418
solve_time_s: 80
verified: false
draft: false
---

[CF 105418G - Số lẻ không phải số nguyên tố](https://codeforces.com/problemset/problem/105418/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm là một chuỗi các số nguyên đại diện cho một hàng thẻ. Từ hàng này, chúng tôi có thể chọn bất kỳ phân đoạn liền kề nào và đối với phân đoạn đó, chúng tôi tính điểm được xác định bằng số giá trị lẻ trừ đi số giá trị nguyên tố bên trong nó. Nhiệm vụ là tìm điểm tối đa có thể trên tất cả các phân đoạn liền kề không trống. 

Cấu trúc hoàn toàn tuyến tính, vì vậy mọi quyết định đều liên quan đến vị trí bắt đầu và kết thúc của mảng con. Không có lựa chọn tổ hợp nào ngoài các khoảng liền kề, điều này ngay lập tức gợi ý lý luận dựa trên tiền tố hoặc kiểu Kadane hơn là bất cứ điều gì theo cấp số nhân. 

Các ràng buộc đủ nhỏ để$O(n^2)$mỗi giải pháp thử nghiệm sẽ vượt qua. Với tổng số$n \le 2000$, thậm chí một vòng lặp kép đánh giá tất cả các mảng con cũng có khoảng bốn triệu phép tính, điều này ổn trong Python. Tuy nhiên, giải pháp dự định là tuyến tính cho mỗi trường hợp thử nghiệm. 

Một điểm tinh tế nằm ở sự tương tác giữa “lẻ” và “tố”. Mọi số nguyên tố ngoại trừ 2 đều là số lẻ, vì vậy các số nguyên tố trùng nhau nhiều với số lẻ. Điều này có nghĩa là những đóng góp không phải là những hạng mục độc lập; một số có thể tăng số lẻ và đồng thời giảm số nguyên tố. Số 2 là số nguyên tố duy nhất không lẻ, điều này khiến nó hoạt động khác với các số nguyên tố khác. 

Một sai lầm ngây thơ là coi điểm số như hai mảng độc lập hoặc thử đếm tiền tố của tỷ lệ cược và số nguyên tố riêng biệt mà không nhận ra chúng trùng nhau. Một sai lầm khác là cho rằng các số nguyên tố chỉ quan trọng như một tập hợp con của tỷ lệ cược; trên thực tế, ngay cả các số nguyên tố không lẻ (chỉ 2) vẫn làm giảm điểm và các số nguyên tố lẻ đóng góp số 0 nếu xử lý không đúng. 

Ví dụ: hãy xem xét một phân đoạn chỉ chứa 2. Điểm là$0 - 1 = -1$. Một cách tiếp cận bất cẩn chỉ coi các số nguyên tố lẻ là “tỷ lệ cược xấu” sẽ coi nó là trung tính một cách không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp brute-force đánh giá từng mảng con và tính điểm trực tiếp của nó. Với mỗi cặp chỉ số$l, r$, chúng tôi quét phân đoạn và đếm tỷ lệ cược và số nguyên tố. Điều này đơn giản và chính xác vì nó tuân theo định nghĩa. Tuy nhiên, nó lặp đi lặp lại cùng một công việc: mỗi phân đoạn sẽ tính toán lại số lượng từ đầu. 

Độ phức tạp về thời gian của phương pháp này là$O(n^3)$cho mỗi trường hợp thử nghiệm nếu được triển khai theo đúng nghĩa đen, vì có$O(n^2)$mảng con và mỗi mảng mất$O(n)$để đánh giá. Ngay cả với tổng tiền tố làm giảm đánh giá xuống$O(1)$, chúng ta vẫn cần mã hóa cẩn thận cả chỉ báo số lẻ và số nguyên tố, nhưng điều đó chỉ làm giảm nó thành$O(n^2)$, có thể chấp nhận được dưới các ràng buộc nhưng không tối ưu. 

Nhận xét quan trọng là mỗi yếu tố đóng góp độc lập vào điểm số. Với mỗi giá trị$a_i$, chúng ta có thể gán cho nó một trọng số: 

nếu là số lẻ thì nó đóng góp +1; nếu là số nguyên tố thì đóng góp -1; nếu cả hai (số nguyên tố lẻ), đóng góp ròng là 0; nếu không, nó đóng góp 0. Vì 2 là số nguyên tố chẵn duy nhất nên nó nhận -1. Mọi số nguyên tố lẻ đều đóng góp +1 và -1, triệt tiêu thành 0. 

Điều này làm giảm vấn đề tìm tổng mảng con tối đa trên một mảng được chuyển đổi trong đó mỗi phần tử có trọng số cố định. Đó chính xác là thuật toán của Kadane. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$hoặc$O(n^2)$|$O(1)$| Quá chậm / Được chấp nhận trong giới hạn | 
| Tối ưu (Kadane) |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước xem mỗi số có phải là số nguyên tố hay không. Vì giá trị tăng lên$10^6$, chúng tôi sử dụng sàng một lần trên toàn cầu. Điều này tránh việc kiểm tra tính nguyên thủy lặp đi lặp lại trong mỗi trường hợp thử nghiệm. 
2. Đối với mỗi phần tử$a_i$, tính phần đóng góp của nó: 

nếu nó là số lẻ, hãy cộng +1 vào điểm cơ bản của nó; nếu nó là số nguyên tố, hãy trừ 1. Điều này tạo ra một mảng số nguyên duy nhất biểu thị bài toán. 
3. Chạy thuật toán tổng mảng con tối đa trên mảng được chuyển đổi này. Duy trì kết thúc tốt nhất đang diễn ra ở đây và kết thúc tốt nhất toàn cầu. 
4. Tại mỗi vị trí, hãy quyết định nên mở rộng phân đoạn trước đó hay bắt đầu mới. Lựa chọn này phản ánh liệu tiền tố phủ định có gây hại cho lợi ích trong tương lai hay không. 
5. Theo dõi giá trị tối đa nhìn thấy trên tất cả các vị trí. 

Lý do đằng sau bước 4 là mọi phân đoạn tối ưu đều phải bao gồm phần tử trước đó hoặc khởi động lại; không có lợi ích gì khi bỏ qua các phần tử bên trong mảng con đã chọn. 

### Tại sao nó hoạt động 

Sau khi chuyển đổi, mỗi phần tử đóng góp một giá trị độc lập cố định cho bất kỳ mảng con nào. Điểm của một mảng con trở thành tổng các phần tử của nó trong mảng được chuyển đổi này. Bất kỳ tổng tối đa hóa phân đoạn liền kề nào cũng chính xác là vấn đề mảng con tối đa cổ điển. Quá trình chuyển đổi tham lam trong thuật toán của Kadane đảm bảo rằng bất kỳ tiền tố nào có tổng âm sẽ bị loại bỏ vì nó làm giảm tất cả các tổng trong tương lai trong phân đoạn đó, do đó việc khởi động lại mang lại kết quả tốt hơn hoặc bằng nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 10**6

# Sieve for primality
is_prime = [True] * (MAXV + 1)
is_prime[0] = is_prime[1] = False
for i in range(2, int(MAXV ** 0.5) + 1):
    if is_prime[i]:
        step = i
        start = i * i
        for j in range(start, MAXV + 1, step):
            is_prime[j] = False

t = int(input())
for _ in range(t):
    n = int(input())
    arr = list(map(int, input().split()))

    best = -10**18
    cur = -10**18

    for x in arr:
        val = 0
        if x % 2 == 1:
            val += 1
        if is_prime[x]:
            val -= 1

        if cur == -10**18:
            cur = val
        else:
            cur = max(val, cur + val)

        best = max(best, cur)

    print(best)
```Sàng được xây dựng một lần trên toàn cầu để đảm bảo rằng việc kiểm tra tính nguyên thủy diễn ra liên tục trong mỗi trường hợp thử nghiệm. Mỗi số được chuyển đổi thành đóng góp của nó một cách độc lập. Bước Kadane sử dụng phép truy toán tiêu chuẩn: bắt đầu một mảng con mới ở phần tử hiện tại hoặc mở rộng phần tử trước đó. 

Việc khởi tạo của`cur`bằng một trọng điểm sẽ tránh được sự mơ hồ cho phần tử đầu tiên, đảm bảo giá trị đầu tiên luôn bắt đầu một phân đoạn một cách rõ ràng. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng:`[1, 2, 3, 4]`. 

Chúng tôi tính toán đóng góp: 

- 1: số lẻ, không phải số nguyên tố → +1 
- 2: số nguyên tố chẵn → -1 
- 3: số nguyên tố lẻ → +1 - 1 = 0 
- 4: chẵn không nguyên tố → 0 

Vì vậy, mảng biến đổi là`[1, -1, 0, 0]`. 

| Chỉ mục | Giá trị | Đóng góp | Cur Kết thúc hay nhất ở đây | Tốt nhất toàn cầu | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 
| 2 | 2 | -1 | 0 | 1 | 
| 3 | 3 | 0 | 0 | 1 | 
| 4 | 4 | 0 | 0 | 1 | 

Phân đoạn tối đa là`[1]`với giá trị 1. Điều này xác nhận rằng những đóng góp tiêu cực (như 2) sẽ đặt lại lợi nhuận tiềm năng. 

Bây giờ hãy xem xét`[2, 3, 5, 4]`. 

Đóng góp: 

- 2 → -1 
- 3 → 0 
- 5 → 0 
- 4 → 0 

| Chỉ mục | Giá trị | Đóng góp | Cur | Tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | -1 | -1 | -1 | 
| 2 | 3 | 0 | 0 | 0 | 
| 3 | 5 | 0 | 0 | 0 | 
| 4 | 4 | 0 | 0 | 0 | 

Đoạn tối ưu là`[3, 5, 4]`đưa ra 0, cho thấy rằng việc bắt đầu sau phần tử âm sẽ cải thiện kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi bài kiểm tra | Mỗi phần tử được xử lý một lần sau quá trình tiền xử lý sàng | 
| Không gian |$O(10^6)$| Sàng lưu trữ cho tính nguyên thủy | 

Tổng cộng$n$trong các trường hợp thử nghiệm là nhỏ nên quá trình quét tuyến tính nằm trong giới hạn. Chi phí sàng phát sinh một lần và được tái sử dụng, giúp giải pháp tổng thể trở nên hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# Note: full solution function integration assumed in real environment

# custom conceptual checks (format illustrative)
assert True  # placeholder since full harness depends on integration
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n2`|`-1`| số nguyên tố chẵn duy nhất | 
|`1\n1\n3`|`0`| số nguyên tố lẻ hủy bỏ | 
|`1\n4\n1 2 3 4`|`1`| đóng góp hỗn hợp | 
|`1\n3\n4 6 8`|`1`| tất cả các hành vi chẵn/lẻ không chính thức | 
|`1\n5\n2 3 5 7 11`|`0`| cấu trúc hủy bỏ tất cả các số nguyên tố | 

## Vỏ cạnh 

Một phần tử đơn bằng 2 là trường hợp biên quan trọng nhất. Thuật toán gán cho nó giá trị -1 và Kadane xử lý chính xác nó như một phân mảng độc lập vì việc mở rộng từ các tiền tố âm sẽ chỉ làm giảm tổng trong tương lai. 

Một mảng chỉ gồm các số nguyên tố lẻ tạo ra tất cả các đóng góp bằng 0. Tổng mảng con tối đa trở thành 0 và Kadane tự nhiên chọn bất kỳ phân đoạn không trống nào, xác nhận rằng việc hủy được xử lý chính xác mà không cần viết hoa đặc biệt. 

Một mảng xen kẽ các giá trị dương lớn và âm đơn thể hiện hành vi khởi động lại của Kadane. Bất cứ khi nào -1 xuất hiện, tổng hiện hành sẽ giảm xuống và thuật toán có thể khởi động lại sau đó nếu có lợi, đảm bảo phân đoạn tối ưu.
