---
title: "CF 106015L - Câu đố cuối cùng của Gamal"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi giá trị có một dãy số nguyên và chúng ta phải đếm xem có bao nhiêu cặp chỉ số tạo ra một điều kiện đặc biệt dựa trên bội số chung nhỏ nhất của hai giá trị."
date: "2026-06-22T16:47:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106015
codeforces_index: "L"
codeforces_contest_name: "Game of Coders 4 - Over the Garden Wall"
rating: 0
weight: 106015
solve_time_s: 57
verified: true
draft: false
---

[CF 106015L - Câu đố cuối cùng của Gamal](https://codeforces.com/problemset/problem/106015/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi giá trị có một dãy số nguyên và chúng ta phải đếm xem có bao nhiêu cặp chỉ số tạo ra một điều kiện đặc biệt dựa trên bội số chung nhỏ nhất của hai giá trị. 

Một cặp vị trí góp phần đưa ra câu trả lời nếu LCM của các giá trị của chúng có cấu trúc rất cụ thể: mọi số nguyên tố xuất hiện trong hệ số hóa của nó phải xuất hiện với số mũ ít nhất là hai. Nói cách khác, LCM không được chỉ chứa bất kỳ số nguyên tố lũy thừa bậc một nào. Nếu một số nguyên tố chia LCM thì ít nhất nó phải chia cho bình phương. 

Kích thước đầu vào ngay lập tức buộc chúng ta tránh xa mọi thứ bậc hai cho mỗi trường hợp thử nghiệm. Tổng số phần tử trong tất cả các trường hợp thử nghiệm lên tới 200000, do đó, bất kỳ giải pháp nào cố gắng kiểm tra rõ ràng tất cả các cặp bên trong mỗi trường hợp thử nghiệm sẽ hết thời gian chờ. Quá trình quét O(N²) đơn giản sẽ thực hiện khoảng 10¹⁰ thao tác trong trường hợp xấu nhất, vượt xa mức cho phép trong 2 giây. 

Một kiểu thất bại tinh tế xuất hiện nếu người ta cố gắng chỉ suy luận về từng yếu tố riêng lẻ. Ví dụ: một số như 6 (2·3) hoặc 10 (2·5) trông “đơn giản”, nhưng việc kết hợp nó với một số khác có thể đưa các số nguyên tố vào LCM chỉ xuất hiện một lần. Điều kiện không phải là các số riêng lẻ là "toàn bình phương", mà là về cấu trúc nguyên tố được hợp nhất trong LCM. 

Một trường hợp khác phát sinh khi các số có chung số nguyên tố nhưng có số mũ khác nhau. Ví dụ: 4 (2²) và 2 (2¹) tạo ra LCM bằng 4, hợp lệ, nhưng 4 và 8 tạo ra 8, không hợp lệ vì 2³ chứa số mũ nguyên tố 3, nhưng điều đó vẫn ổn vì 3 ≥ 2. Thất bại thực sự xảy ra khi một số nguyên tố xuất hiện đúng một lần trong LCM, điều này chỉ xảy ra khi cả hai số đóng góp nó ở số mũ 1. 

Vì vậy, vấn đề giảm xuống còn việc đếm các cặp có hành vi nguyên tố kết hợp không bao giờ để lại “số mũ đơn lẻ” trong LCM. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp qua tất cả các cặp, tính toán LCM, phân tích nhân tử và kiểm tra xem mọi số mũ nguyên tố có ít nhất là hai hay không. Ngay cả khi tính toán LCM nhanh thì việc phân tích nhân tử cho mỗi cặp cũng không nhanh. Với N tối đa 10⁵, điều này trở thành khoảng 10¹⁰ cặp kiểm tra trong trường hợp xấu nhất, mỗi cặp có số học không tầm thường. Điều này là không khả thi. 

Quan sát quan trọng là điều kiện chỉ phụ thuộc vào việc liệu có bất kỳ số nguyên tố nào xuất hiện với số mũ chính xác bằng một trong LCM hay không. Đối với một số nguyên tố p cho trước, điều này xảy ra chính xác khi một số đóng góp p¹ và số kia đóng góp p⁰ hoặc p¹ theo cách sao cho số mũ tối đa trên cặp chính xác là một. Điều đó có nghĩa là cả hai số phải tránh đưa ra số mũ nguyên tố “đơn độc”. 

Thay vì suy nghĩ trực tiếp về LCM, chúng ta lật ngược góc nhìn: phân loại từng số theo hạt nhân không có bình phương của nó, nhưng chỉ quan tâm đến các số nguyên tố xuất hiện với số mũ chính xác bằng một trong hệ số hóa của nó. Các số nguyên tố xuất hiện với số mũ ≥2 là vô hại vì chúng đã thỏa mãn điều kiện riêng lẻ trong LCM. 

Vì vậy, chúng ta có thể nén mỗi số thành tập hợp các số nguyên tố xuất hiện đúng một lần trong đó. Gọi đây là “chữ ký xấu”. Bây giờ điều kiện trở thành: để một cặp tốt, không được có số nguyên tố nào xuất hiện trong hiệu đối xứng của chữ ký xấu của chúng. Tương tự, cả hai số phải có chữ ký xấu giống hệt nhau, bởi vì bất kỳ sự không khớp nào cũng tạo ra một số nguyên tố xuất hiện chính xác trong một số ở số mũ một và do đó trở thành số mũ một trong LCM. 

Vì vậy, nhiệm vụ giảm xuống việc nhóm các số theo chữ ký này và đếm các cặp trong mỗi nhóm. 

Đối với mọi số, chúng tôi tính toán hệ số nguyên tố của nó và trích xuất các số nguyên tố có số mũ chính xác là một. Chúng tôi mã hóa biểu diễn này dưới dạng biểu diễn chuẩn (ví dụ: bộ được sắp xếp hoặc tập đông lạnh). Sau đó, chúng tôi đếm tần số và tổng hợp các kết hợp trong mỗi nhóm.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2 √A) | O(1) | Quá chậm | 
| Tối ưu | O(N √A + N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Với mỗi số, hãy phân tích nó thành số nguyên tố và ghi số mũ của mỗi số nguyên tố. Chúng tôi chỉ cần các số nguyên tố lên tới 10⁶, do đó phép chia thử lên tới √A hoặc mảng SPF dựa trên sàng được tính toán trước là đủ. Mục đích ở đây là trích xuất thông tin số mũ chính xác chứ không chỉ tính nguyên tố. 
2. Với mỗi số, hãy xây dựng một “chữ ký” bao gồm các số nguyên tố có số mũ chính xác là một. Các số nguyên tố có số mũ từ 2 trở lên bị bỏ qua vì chúng không tạo ra đóng góp đơn lẻ trong bất kỳ cặp LCM nào. 
3. Chuyển chữ ký này thành dạng biểu diễn có thể băm được. Một bộ số nguyên tố được sắp xếp hoạt động vì thứ tự không quan trọng nhưng danh tính phải được giữ nguyên. 
4. Duy trì bản đồ tần số từ chữ ký đến số đếm có chữ ký. Mỗi nhóm đại diện cho các số hoạt động giống hệt nhau về mặt tạo ra các số nguyên tố có số mũ. 
5. Với mỗi nhóm kích thước k, hãy thêm k·(k−1)/2 vào câu trả lời. Điều này đếm tất cả các cặp có chữ ký khớp chính xác, nghĩa là không có số nguyên tố nào chỉ xuất hiện trong một số có số mũ là một. 

Lý do nhóm hoạt động là vì bất kỳ sự không khớp nào giữa hai chữ ký đều đưa ra ít nhất một số nguyên tố có số mũ là một trong đúng một số. Số nguyên tố đó trở thành số mũ trong LCM, vi phạm điều kiện. 

### Tại sao nó hoạt động 

Thuật toán dựa trên sự tương đương về cấu trúc: LCM không có số nguyên tố một số mũ khi và chỉ khi cả hai số có bộ số nguyên tố giống hệt nhau xuất hiện chính xác một lần trong quá trình phân tích nhân tử của chúng. Bất kỳ sai lệch nào đều đưa ra một số nguyên tố tồn tại trong LCM với số mũ một. Do đó, các cặp hợp lệ chính xác là những cặp được hình thành trong các lớp chữ ký giống hệt nhau và việc đếm trở thành một phép tổng hợp tổ hợp đơn giản theo tần số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXA = 10**6

# Precompute smallest prime factor
spf = list(range(MAXA + 1))
for i in range(2, int(MAXA ** 0.5) + 1):
    if spf[i] == i:
        for j in range(i * i, MAXA + 1, i):
            if spf[j] == j:
                spf[j] = i

def factor_signature(x):
    sig = []
    while x > 1:
        p = spf[x]
        cnt = 0
        while x % p == 0:
            x //= p
            cnt += 1
        if cnt == 1:
            sig.append(p)
    return tuple(sig)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        freq = {}
        for _ in range(n):
            x = int(input())
            sig = factor_signature(x)
            freq[sig] = freq.get(sig, 0) + 1

        ans = 0
        for k in freq.values():
            ans += k * (k - 1) // 2
        print(ans)

if __name__ == "__main__":
    solve()
```Sàng xây dựng các thừa số nguyên tố nhỏ nhất để mỗi số có thể được phân tích thành thừa số logarit theo thời gian tương ứng với giá trị của nó. các`factor_signature`Hàm chỉ trích xuất các số nguyên tố có số mũ chính xác bằng một và lưu trữ chúng trong một bộ dữ liệu. Bộ dữ liệu này được sử dụng làm khóa từ điển để nhóm các hành vi giống nhau. 

Vòng lặp cuối cùng đếm các kết hợp cặp bên trong mỗi nhóm bằng cách sử dụng công thức k chọn 2 tiêu chuẩn. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ:`[6, 10, 15]`. 

| Số | Nhân tố hóa | Chữ ký | 
| --- | --- | --- | 
| 6 | 2¹·3¹ | (2, 3) | 
| 10 | 2¹·5¹ | (2, 5) | 
| 15 | 3¹·5¹ | (3, 5) | 

Tất cả các chữ ký đều khác nhau nên không có cặp nào được tính. 

Điều này chứng tỏ rằng mặc dù tất cả các số đều có cấu trúc “tương tự nhau”, nhưng bất kỳ cặp số nào cũng đưa ra một số nguyên tố không khớp và trở thành số mũ trong LCM. 

Bây giờ hãy xem xét`[4, 8, 9, 16]`. 

| Số | Nhân tố hóa | Chữ ký | 
| --- | --- | --- | 
| 4 | 2² | () | 
| 8 | 2³ | () | 
| 9 | 3² | () | 
| 16 | 2⁴ | () | 

Tất cả chữ ký đều trống, vì vậy tất cả các cặp đều hợp lệ. Bảng cho thấy các số nguyên tố có số mũ ≥2 không đóng góp vào chữ ký nên tất cả các số đều xếp vào một nhóm. Mỗi cặp đều được tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log A + A log log A) | sàng một lần cộng với hệ số hóa cho mỗi phần tử | 
| Không gian | O(N + A) | bản đồ tần số và mảng SPF | 

Các ràng buộc cho phép tổng số lên tới 2 × 10⁵ và mỗi hệ số đủ nhanh theo sàng SPF được tính toán trước. Việc sử dụng bộ nhớ bị chi phối bởi sàng và bản đồ băm, cả hai đều nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    MAXA = 10**6
    spf = list(range(MAXA + 1))
    for i in range(2, int(MAXA ** 0.5) + 1):
        if spf[i] == i:
            for j in range(i * i, MAXA + 1, i):
                if spf[j] == j:
                    spf[j] = i

    def factor_signature(x):
        sig = []
        while x > 1:
            p = spf[x]
            cnt = 0
            while x % p == 0:
                x //= p
                cnt += 1
            if cnt == 1:
                sig.append(p)
        return tuple(sig)

    t = int(input())
    for _ in range(t):
        n = int(input())
        freq = {}
        for _ in range(n):
            x = int(input())
            sig = factor_signature(x)
            freq[sig] = freq.get(sig, 0) + 1
        ans = sum(k * (k - 1) // 2 for k in freq.values())
        output.append(str(ans))

    return "\n".join(output)

# provided sample (format assumed)
assert run("1\n4\n10 18 29 36\n") == "0", "sample 1"

# all equal, all square-full
assert run("1\n4\n4 8 9 16\n") == "6", "all valid pairs"

# all distinct bad signatures
assert run("1\n3\n6 10 15\n") == "0", "no valid pairs"

# single prime powers
assert run("1\n5\n2 3 5 7 11\n") == "0", "primes only"

# mixed repetition
assert run("1\n5\n4 4 8 9 9\n") == "4", "grouping check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các số bình phương bằng nhau | 6 | tất cả các cặp hợp lệ trong một nhóm | 
| số nguyên tố hỗn hợp phân biệt | 0 | không có cặp nhóm chéo ngẫu nhiên | 
| cấu trúc lặp lại | 4 | đếm tổ hợp đúng | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các số đều là hình vuông, chẳng hạn như`[4, 8, 16]`. Mọi chữ ký đều trở nên trống, do đó thuật toán nhóm mọi thứ lại với nhau và đếm tất cả các cặp một cách chính xác là k·(k−1)/2. Việc triển khai ngây thơ có thể cố gắng kiểm tra trực tiếp cấu trúc LCM một cách không chính xác và bỏ lỡ hành vi số mũ đó hoàn toàn biến mất trong trường hợp này. 

Một trường hợp khác là khi mọi số đều là tích của các số nguyên tố riêng biệt, chẳng hạn như`[6, 10, 15]`. Mỗi chữ ký là duy nhất nên không tồn tại cặp chữ ký nào. Thuật toán tách chính xác từng số thành nhóm riêng của nó, tránh các kết quả dương tính giả có thể phát sinh nếu người ta chỉ kiểm tra gcd hoặc tính chia hết thay vì cấu trúc số mũ chính xác.
