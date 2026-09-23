---
title: "CF 104787J - Keyi Thích Đọc Sách"
description: "Chúng ta được cung cấp một tập hợp các từ, nhưng đặc tính duy nhất quan trọng của mỗi từ là độ dài của nó. Mỗi ngày Keyi chọn một số từ để học và có một nguyên tắc đặc biệt: nếu cô quyết định học một từ có độ dài $k$ thì ngày hôm đó cô phải học tất cả các từ có độ dài $k$."
date: "2026-06-28T14:23:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104787
codeforces_index: "J"
codeforces_contest_name: "The 2023 CCPC (Qinhuangdao) Onsite (The 2nd Universal Cup. Stage 9: Qinhuangdao)"
rating: 0
weight: 104787
solve_time_s: 47
verified: true
draft: false
---

[CF 104787J - Keyi Thích Đọc](https://codeforces.com/problemset/problem/104787/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các từ, nhưng đặc tính duy nhất quan trọng của mỗi từ là độ dài của nó. Mỗi ngày, Keyi chọn một số từ để học và có một quy tắc đặc biệt: nếu cô quyết định học một từ dài$k$, sau đó cô ấy phải học tất cả các từ có độ dài$k$ngày hôm đó. Nói cách khác, độ dài hoạt động giống như các nhóm không thể phân chia được. 

Mỗi ngày đều có giới hạn sức chứa$W$, nghĩa là tổng số từ học ngày hôm đó không thể vượt quá$W$. Vì việc chọn một độ dài buộc tất cả các từ có độ dài đó nên quyết định thực sự là độ dài nào sẽ được gói lại với nhau trong cùng một ngày, với tổng tần suất của chúng không vượt quá$W$. 

Đầu vào mang lại$n$độ dài từ, mỗi từ trong phạm vi từ 1 đến 13. Nhiệm vụ là phân chia các độ dài này thành số ngày hợp lệ tối thiểu, trong đó mỗi ngày là tập hợp con của các nhóm độ dài và tổng kích thước nhóm trong một ngày không vượt quá$W$. 

Đầu ra là số ngày tối thiểu cần thiết để bao gồm tất cả các từ. 

Những hạn chế quan trọng theo một cách rất cụ thể. Mặc dù$n$có thể lớn tới 50000, không gian giá trị của độ dài rất nhỏ, giới hạn bởi 13. Điều này ngay lập tức gợi ý rằng mọi giải pháp tùy thuộc vào số lượng độ dài riêng biệt đều nhỏ và có thể quản lý được. Một giải pháp theo dõi số lượng trên mỗi độ dài là đủ; việc lặp qua tất cả các tập con có độ dài vẫn khả thi vì có nhiều nhất$2^{13}$khả năng. 

Một cách giải thích ngây thơ có thể cố gắng gán từng từ một hoặc mô phỏng việc đóng gói hàng ngày một cách tham lam mà không xem xét cấu trúc tổng thể. Điều này dễ dàng bị phá vỡ vì các quyết định về nhóm phụ thuộc lẫn nhau: việc chọn một nhóm lớn sớm có thể cản trở việc đóng gói tốt hơn sau này. 

Trường hợp khó nhận biết xuất hiện khi một chiều dài chiếm ưu thế về dung lượng: 

đầu vào: 

n = 5, W = 4 

độ dài: [1, 1, 1, 1, 1] 

Mỗi nhóm độ dài chỉ có độ dài 1 với tần số 5. Vì chúng ta phải gộp tất cả chúng lại với nhau nhưng không thể vượt quá 4 mỗi ngày nên chúng ta cần ít nhất 2 ngày. Bất kỳ cách tiếp cận tham lam nào cố gắng “khớp các từ còn lại” mà không tôn trọng ràng buộc nhóm có thể bị chia tách hoặc đếm thiếu một cách không chính xác. 

Một trường hợp đặc biệt khác là khi nhiều nhóm nhỏ kết hợp chặt chẽ để lấp đầy chính xác số ngày thay vì vượt quá công suất một chút, trong đó việc đóng gói tối ưu phụ thuộc vào việc lựa chọn tập hợp con thay vì đặt hàng. 

## Phương pháp tiếp cận 

Điểm trừu tượng chính là quên các từ riêng lẻ và thay vào đó nén đầu vào thành số lượng tần số trên mỗi độ dài. Vì độ dài chỉ nằm trong khoảng từ 1 đến 13 nên chúng tôi nhận được tối đa 13 nhóm, mỗi nhóm có kích thước cố định. 

Bây giờ vấn đề trở thành: chúng tôi có tối đa 13 mục, mỗi mục có trọng lượng bằng tần số của nó và chúng tôi muốn đóng gói tất cả các mục vào số thùng có sức chứa tối thiểu$W$, với ràng buộc là các mục không thể chia được. 

Đây chính xác là một vấn đề về việc đóng thùng với số lượng vật phẩm rất nhỏ nhưng có thể có trọng lượng lớn. Ý tưởng vũ lực sẽ mô phỏng việc phân công các nhóm theo ngày, thử tất cả các nhiệm vụ có thể. Điều đó dẫn đến việc tìm kiếm theo cấp số nhân trên các vị trí, đại khái là$O(k^n)$Ở đâu$k$là số lượng thùng, không thể thực hiện được ngay cả đối với đầu vào vừa phải. 

Tuy nhiên, vì có tối đa 13 nhóm riêng biệt nên chúng ta có thể sử dụng lập trình động bitmask trên các tập hợp con của nhóm. Mỗi trạng thái đại diện cho nhóm nào đã được đóng gói và chúng tôi cố gắng lấp đầy một ngày bằng cách chọn một tập hợp con gồm các nhóm còn lại có tổng kích thước không vượt quá$W$. Đối với mỗi tập hợp con hợp lệ, chúng tôi chuyển đổi bằng cách đánh dấu các nhóm đó là đã sử dụng và thêm một ngày. 

Điều này biến bài toán thành đường đi ngắn nhất trên các tập hợp con, trong đó mỗi lần di chuyển tương ứng với việc lấp đầy một ngày một cách tối ưu. 

Quan sát quan trọng là việc đóng gói một ngày không phụ thuộc vào những ngày trước đó, ngoại trừ các nhóm chưa sử dụng còn lại. Vì vậy, mỗi lần chuyển đổi DP thể hiện đầy đủ một ngày làm việc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân công vũ lực | Hàm mũ | O(n) | Quá chậm | 
| Bitmask DP trên các tập hợp con |$O(2^{13} \cdot 2^{13})$hoặc tối ưu hóa$O(2^{13} \cdot 13)$|$O(2^{13})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén đầu vào thành một mảng tần số`cnt`có kích thước 13, ở đâu`cnt[i]`là số từ có độ dài$i+1$. 

Sau đó, chúng tôi coi mỗi nhóm trong số 13 nhóm này là một mục có trọng lượng`cnt[i]`. 

Chúng tôi xác định một bitmask DP trong đó mỗi mặt nạ đại diện cho nhóm độ dài nào đã được gán đầy đủ cho ngày. 

Chúng tôi tính toán DP[mask] là số ngày tối thiểu cần thiết để đóng gói tất cả các nhóm trong`mask`. 

### Các bước 

1. Tính toán`cnt[0..12]`từ đầu vào. 
2. Tính toán trước tất cả các tập hợp con hợp lệ của nhóm. Một tập hợp con hợp lệ nếu tổng số đếm của chúng không vượt quá$W$. Điều này thể hiện việc đóng gói trong một ngày. 
3. Khởi tạo mảng DP có kích thước$2^{13}$với giá trị lớn, đặt DP[0] = 0. 
4. Lặp lại tất cả các mặt nạ từ 0 đến$2^{13}-1$. 
5. Đối với mỗi mặt nạ, hãy xem xét tất cả các tập hợp con của các nhóm chưa sử dụng còn lại. 
6. Nếu một tập hợp con hợp lệ và tách rời khỏi mặt nạ hiện tại, hãy chuyển sang mặt nạ mới bằng cách thêm 1 ngày. 
7. Tận dụng tối thiểu tất cả các chuyển đổi. 
8. Đáp án là DP[(1<<13)-1]. 

Quyết định thiết kế quan trọng là tính toán trước các tập hợp con hợp lệ, vì việc kiểm tra trọng số nhiều lần bên trong DP sẽ nhân thời gian chạy một cách không cần thiết. Vì 13 là nhỏ nên việc liệt kê tất cả các tập hợp con là khả thi. 

### Tại sao nó hoạt động 

Ở bất kỳ trạng thái DP nào, mặt nạ mã hóa chính xác những nhóm còn lại. Mỗi lần chuyển đổi sẽ chọn một tập hợp các nhóm còn lại có thể phù hợp với một ngày. Bởi vì các nhóm không thể phân chia được và phải được lên lịch đầy đủ nên mỗi nhóm xuất hiện chính xác một lần trong tất cả các lần chuyển tiếp. DP khám phá tất cả các phân vùng có thể có của 13 nhóm vào các thùng chứa dung lượng$W$và vì mỗi phân vùng hợp lệ tương ứng với một số chuỗi xóa tập hợp con, nên giá trị tối thiểu trên DP sẽ nắm bắt được số ngày tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, W = map(int, input().split())
    a = list(map(int, input().split()))

    cnt = [0] * 13
    for x in a:
        cnt[x - 1] += 1

    # Precompute subset weights
    m = 13
    subset_weight = [0] * (1 << m)
    for mask in range(1 << m):
        s = 0
        for i in range(m):
            if mask & (1 << i):
                s += cnt[i]
        subset_weight[mask] = s

    valid = []
    for mask in range(1 << m):
        if subset_weight[mask] <= W:
            valid.append(mask)

    INF = 10**9
    dp = [INF] * (1 << m)
    dp[0] = 0

    full = (1 << m) - 1

    for mask in range(1 << m):
        if dp[mask] == INF:
            continue
        remaining = full ^ mask
        for sub in valid:
            if (sub & mask) == 0:
                new_mask = mask | sub
                if dp[new_mask] > dp[mask] + 1:
                    dp[new_mask] = dp[mask] + 1

    print(dp[full])

if __name__ == "__main__":
    main()
```Quá trình triển khai bắt đầu bằng cách nén đầu vào thành dải tần số có độ dài 13. Bước này rất quan trọng vì nó làm giảm một lượng lớn$n$vấn đề vào một không gian trạng thái có kích thước cố định. 

các`subset_weight`tính toán là bước tiền xử lý cốt lõi. Nó đánh giá mọi tập hợp con có độ dài và tính toán số lượng từ sẽ được bao gồm nếu các độ dài đó được chọn cùng nhau trong một ngày. 

các`valid`liệt kê chỉ lưu trữ các tập hợp con phù hợp với giới hạn hàng ngày$W$. Điều này tránh việc kiểm tra dung lượng lặp đi lặp lại trong quá trình chuyển đổi DP. 

Vòng lặp DP lặp qua các mặt nạ theo thứ tự tăng dần. Đối với mỗi trạng thái, nó cố gắng thêm một tập hợp con hợp lệ của các nhóm không sử dụng. Kiểm tra XOR đảm bảo chúng tôi không bao giờ sử dụng lại một nhóm. Mỗi lần chuyển đổi tương ứng với việc tiêu thụ đúng một ngày. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4
1 2 1 2 1
```Đếm:`cnt = [3, 2, 0, ..., 0]`Chúng ta có hai nhóm: chiều dài 1 có kích thước 3, chiều dài 2 có kích thước 2. 

| Mặt nạ | Các nhóm được bao gồm | Cân nặng | DP | 
| --- | --- | --- | --- | 
| 0000 | không | 0 | 0 | 
| 0001 | {len1} | 3 | 1 | 
| 0010 | {len2} | 2 | 1 | 
| 0011 | {len1,len2} | 5 (không hợp lệ) | INF | 

Từ trạng thái 0, chúng ta có thể chọn riêng một trong hai nhóm. Việc lấy cả hai đều không hợp lệ vì 5 > 4. Từ trạng thái một nhóm, nhóm còn lại sẽ được lấy vào một ngày khác. 

Trả lời: 2 

Điều này cho thấy DP thực thi chính xác các hạn chế về năng lực và ngăn chặn việc kết hợp các nhóm vượt quá$W$. 

### Ví dụ 2 

đầu vào:```
6 6
1 1 1 2 2 2
```Đếm:`cnt[0]=3, cnt[1]=3`| Mặt nạ | Hành động | DP | 
| --- | --- | --- | 
| 00 | bắt đầu | 0 | 
| 01 | lấy len1 | 1 | 
| 10 | đưa len2 | 1 | 
| 11 | không thể lấy cả hai (3+3=6 hợp lệ, thực sự hợp lệ) | 1 | 

Ở đây cả hai nhóm đều khớp chính xác trong một ngày, vì vậy DP tìm thấy câu trả lời tối ưu 1. 

Điều này chứng tỏ rằng thuật toán khai thác các cơ hội đóng gói chặt chẽ một cách tự nhiên thay vì tách nhóm một cách tham lam. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^{13} \cdot 2^{13})$| DP trên tất cả các mặt nạ, thử tất cả các tập hợp con hợp lệ | 
| Không gian |$O(2^{13})$| Mảng DP trên các tập hợp con | 

Không gian trạng thái được cố định ở mức 8192 và các chuyển tiếp được giới hạn bởi cùng một hệ số không đổi. Điều này nằm trong giới hạn thoải mái cho giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, W = map(int, input().split())
    a = list(map(int, input().split()))

    cnt = [0] * 13
    for x in a:
        cnt[x - 1] += 1

    m = 13
    subset_weight = [0] * (1 << m)
    for mask in range(1 << m):
        s = 0
        for i in range(m):
            if mask & (1 << i):
                s += cnt[i]
        subset_weight[mask] = s

    valid = [mask for mask in range(1 << m) if subset_weight[mask] <= W]

    INF = 10**9
    dp = [INF] * (1 << m)
    dp[0] = 0

    full = (1 << m) - 1

    for mask in range(1 << m):
        if dp[mask] == INF:
            continue
        for sub in valid:
            if (sub & mask) == 0:
                dp[mask | sub] = min(dp[mask | sub], dp[mask] + 1)

    return str(dp[full])

# sample
assert run("5 4\n1 2 1 2 1\n") == "2"

# all same
assert run("4 4\n1 1 1 1\n") == "1"

# tight split
assert run("6 4\n1 1 1 2 2 2\n") == "2"

# minimal
assert run("1 1\n1\n") == "1"

# each must be separate
assert run("3 1\n1 2 3\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều có cùng độ dài | 1 | tập hợp đầy đủ trong một ngày | 
| hỗn hợp đóng gói chặt chẽ | 2 | tối ưu hóa việc đóng gói tập hợp con | 
| W = 1 trường hợp | n | buộc phải chia tay | 
| phần tử đơn | 1 | trường hợp cơ sở | 

## Vỏ cạnh 

Trường hợp cạnh khóa xuất hiện khi một nhóm độ dài vượt quá một nửa$W$, khiến nó không thể kết hợp với hầu hết các nhóm khác. Ví dụ: nếu một nhóm có kích thước 5 và$W = 6$, chỉ những kết hợp rất cụ thể mới hợp lệ. DP xử lý việc này một cách tự nhiên vì tính hợp lệ của tập hợp con được kiểm tra hoàn toàn bằng ràng buộc tổng, không phụ thuộc vào thứ tự tham lam. 

Một trường hợp khác là khi nhiều kết hợp khớp chính xác$W$. Trong những trường hợp như vậy, chiến lược tham lam có thể chọn một tập hợp con dưới mức tối ưu trước và phân chia các nhóm còn lại. DP tránh điều này vì nó khám phá tất cả các tập hợp con hợp lệ một cách đối xứng và luôn giảm thiểu tổng số ngày từ mọi trạng thái mặt nạ có thể tiếp cận. 

Một trường hợp tinh tế cuối cùng là khi tất cả các nhóm đều nhỏ nhưng về tổng thể lại vượt quá năng lực theo nhiều cách, khiến cho việc đóng gói có tính tổ hợp cao. Ngay cả ở đây, việc liệt kê tập hợp con đảm bảo tất cả các gói hợp lệ đều được xem xét và phân vùng tối ưu được tìm thấy thông qua việc giảm thiểu trạng thái thay vì thứ tự xây dựng.
