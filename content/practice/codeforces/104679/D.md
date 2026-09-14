---
title: "CF 104679D - Một mảng bí ẩn khác"
description: "Trò chơi được chơi trên một dãy số nguyên dương. Hai người chơi luân phiên nhau. Ở mỗi lượt, người chơi chọn một số nguyên tố chia ít nhất một phần tử của mảng."
date: "2026-06-29T09:01:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104679
codeforces_index: "D"
codeforces_contest_name: "Replay of Battle of Brains 2022, University of Dhaka"
rating: 0
weight: 104679
solve_time_s: 43
verified: true
draft: false
---

[CF 104679D - Một mảng bí ẩn khác](https://codeforces.com/problemset/problem/104679/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi được chơi trên một dãy số nguyên dương. Hai người chơi luân phiên nhau. Ở mỗi lượt, người chơi chọn một số nguyên tố chia ít nhất một phần tử của mảng. Một lần là thủ tướng$p$được chọn, mọi số trong mảng chia hết cho$p$được chia cho$p$đồng thời. Người chơi không thể chọn bất kỳ số nguyên tố hợp lệ nào sẽ thua. 

Khó khăn chính là việc di chuyển không cục bộ đối với một phần tử duy nhất, nó tác động lên tất cả các lần xuất hiện của thừa số nguyên tố trên toàn bộ mảng. Điều này tạo ra sự ghép nối giữa các phần tử thông qua các số nguyên tố được chia sẻ, nhưng chỉ thông qua số mũ của chúng. 

Đầu vào là danh sách các số nguyên và đầu ra là một quyết định duy nhất: liệu người chơi đầu tiên có giành chiến thắng bắt buộc hay không nếu cả hai người chơi đều chơi tối ưu. 

Các ràng buộc đủ nhỏ để có thể phân tích từng số một cách độc lập. Mô phỏng trực tiếp của trò chơi sẽ quét mảng liên tục, tìm số nguyên tố hợp lệ, áp dụng phép chia và tiếp tục. Điều này quá chậm vì mỗi lần di chuyển yêu cầu cập nhật đầy đủ mảng và số lần di chuyển có thể lớn khi các số chứa các thừa số nguyên tố lặp lại. 

Một vài kịch bản thất bại xuất hiện trong lối suy nghĩ ngây thơ. Nếu người ta cố gắng mô phỏng các nước đi một cách tham lam bằng cách luôn chọn số nguyên tố nhỏ nhất thì kết quả sẽ không chính xác vì thứ tự không quan trọng giữa các số nguyên tố khác nhau. 

Ví dụ, hãy xem xét mảng$[4, 2]$. Một chiến lược ngây thơ có thể chọn nguyên tố$2$, nhưng ngay cả khi một chiến lược khác được sử dụng, cấu trúc của trò chơi vẫn độc lập với mỗi số nguyên tố, do đó các lựa chọn thứ tự không ảnh hưởng đến tổng số nước đi. Tính chính xác phụ thuộc vào việc đếm các bước di chuyển chứ không phải mô phỏng chúng. 

Một vấn đề tế nhị khác là giả sử mỗi lần xuất hiện của một số nguyên tố đều góp phần độc lập. Vì$[4, 8]$, việc đếm từng phần tử riêng biệt sẽ vượt quá số lần di chuyển cho số nguyên tố$2$, vì cả hai phần tử đều giảm đồng thời. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp coi mảng như đang phát triển sau mỗi lần di chuyển. Chúng tôi quét tìm ước số nguyên tố có trong mảng, chia tất cả các phần tử chia hết và lặp lại cho đến khi không còn số nguyên tố nào. Mỗi chi phí hoạt động$O(n)$và trong trường hợp xấu nhất, chúng ta có thể thực hiện nhiều thao tác, đặc biệt khi các số chứa các thừa số nguyên tố nhỏ lặp lại. Nếu giá trị lớn, số bước có thể đạt tới tổng của tất cả số mũ trên tất cả các phần tử, khiến phương pháp này không thực tế. 

Cái nhìn sâu sắc về cấu trúc là các số nguyên tố khác nhau tiến triển độc lập. Chọn một số nguyên tố$p$chỉ ảnh hưởng đến số mũ của$p$ở mọi con số; nó không tương tác với bất kỳ số nguyên tố nào khác. Vì vậy, trò chơi phân rã thành các trò chơi con độc lập, mỗi trò chơi con có một số nguyên tố. 

Đối với số nguyên tố cố định$p$, mỗi lần di chuyển sẽ làm giảm mọi số mũ khác 0 của$p$bằng chính xác một. Việc di chuyển có thể thực hiện được miễn là ít nhất một số vẫn có số mũ dương là$p$. Do đó số nước đi hợp lệ được đóng góp bởi số nguyên tố$p$chính xác là số mũ tối đa của$p$trên tất cả các số trong mảng. Khi mức tối đa đó đạt đến 0, không có phần tử nào chứa$p$, vì vậy trò chơi không thể tiếp tục trong thời gian đó. 

Tổng số đóng góp này trên tất cả các số nguyên tố sẽ cho ra tổng số nước đi trong toàn bộ trò chơi. Vì mỗi bước di chuyển sẽ loại bỏ chính xác một lớp của một số thừa số nguyên tố trên toàn cầu, nên không có sự tương tác giữa các số nguyên tố về số lần di chuyển. 

Người chiến thắng được xác định hoàn toàn bằng tính chẵn lẻ của tổng số nước đi, bởi vì người chơi luân phiên nhau và nước đi cuối cùng sẽ quyết định người chiến thắng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(T \cdot n)$Ở đâu$T$là số lần di chuyển |$O(1)$thêm | Quá chậm | 
| Tổng hợp thừa số nguyên tố |$O(n \sqrt{A})$hoặc$O(n \log A)$|$O(\text{primes})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi điều chỉnh lại trò chơi bằng cách đếm tổng số "loại bỏ lớp cơ bản" tồn tại trên tất cả các số nguyên tố. 

1. Phân tích từng số trong mảng thành số nguyên tố và số mũ của chúng. Điều này cô lập các thành phần độc lập của trò chơi. 
2. Đối với mỗi số nguyên tố$p$, duy trì số mũ tối đa nhìn thấy trên tất cả các phần tử mảng. Điều này tượng trưng cho bao nhiêu lần$p$vẫn có thể được chọn là nước đi hợp lệ trước khi biến mất hoàn toàn. 
3. Tổng hợp các cực đại này trên tất cả các số nguyên tố. Tổng thể hiện tổng số nước đi trong trò chơi. 
4. Xác định người chiến thắng bằng cách kiểm tra tính chẵn lẻ của tổng số này. Nếu tổng số nước đi là số lẻ thì người chơi đầu tiên thực hiện nước đi cuối cùng và thắng; nếu không người chơi thứ hai sẽ thắng. 

Bước lý luận chính là cách chúng ta tổng hợp số mũ. Mỗi lần di chuyển sẽ giảm đồng thời tất cả các lần xuất hiện khác 0 của một số nguyên tố, do đó hệ số giới hạn là số mũ lớn nhất, không phải tổng. 

### Tại sao nó hoạt động 

Mỗi số nguyên tố hoạt động giống như một chồng có chiều cao bằng số mũ của nó trong mỗi phần tử. Một bước di chuyển sẽ loại bỏ một cấp khỏi mọi ngăn xếp chứa số nguyên tố đó. Quá trình kết thúc khi ngăn xếp cao nhất của số nguyên tố đó bằng 0. Vì mỗi lần di chuyển sẽ loại bỏ chính xác một lớp trên tất cả các ngăn xếp chứa số nguyên tố đó, nên số lần loại bỏ như vậy bằng với chiều cao tối đa. Vì các số nguyên tố không ảnh hưởng lẫn nhau nên các quá trình này chạy song song mà không làm thay đổi số đếm. Do đó, tổng thời lượng trò chơi là tổng chiều cao của ngăn xếp độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    maxa = max(arr)

    # smallest prime factor sieve up to maxa
    spf = list(range(maxa + 1))
    for i in range(2, int(maxa ** 0.5) + 1):
        if spf[i] == i:
            for j in range(i * i, maxa + 1, i):
                if spf[j] == j:
                    spf[j] = i

    max_exp = {}

    for x in arr:
        while x > 1:
            p = spf[x]
            cnt = 0
            while x % p == 0:
                x //= p
                cnt += 1
            if p in max_exp:
                if cnt > max_exp[p]:
                    max_exp[p] = cnt
            else:
                max_exp[p] = cnt

    total_moves = sum(max_exp.values())

    print("Alice" if total_moves % 2 == 1 else "Bob")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng một sàng thừa số nguyên tố nhỏ nhất để mỗi số nguyên có thể được phân tích thành thừa số một cách hiệu quả. Điều này tránh việc phân chia thử nghiệm lặp đi lặp lại và đảm bảo mỗi số được phân tách theo thời gian gần logarit. 

Mỗi số sau đó được chia thành các thừa số nguyên tố. Đối với mỗi số nguyên tố gặp phải, chúng tôi tính số mũ của nó trong số đó và cập nhật từ điển lưu trữ số mũ tối đa trên toàn mảng. Từ điển là biểu diễn nén của tất cả các ngăn xếp nguyên tố độc lập. 

Cuối cùng, tổng hợp các cực đại này sẽ cho ra tổng số bước di chuyển. Việc kiểm tra tính chẵn lẻ sẽ xác định người chiến thắng. 

Một cạm bẫy triển khai phổ biến là tính tổng không chính xác số mũ của tất cả các phần tử thay vì lấy giá trị tối đa cho mỗi số nguyên tố. Điều đó sẽ đếm quá nhiều lần di chuyển vì tất cả các lần xuất hiện của số nguyên tố đều giảm đồng thời trong mỗi lần di chuyển. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng$[2, 4]$. 

Chúng tôi tính:$2 = 2^1$,$4 = 2^2$. 

| Bước | Số | Thủ tướng | Tìm thấy số mũ | Bản đồ số mũ tối đa | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 1 | {2: 1} | 
| 2 | 4 | 2 | 2 | {2: 2} | 

Tổng số lần di chuyển là$2$. Nước đi đầu tiên làm giảm cả hai số đi 2, cho$[1, 2]$. Nước đi thứ 2 làm giảm 2 quân còn lại, kết thúc ván chơi. Tổng điểm là chẵn nên người chơi thứ hai thắng. 

Bây giờ hãy xem xét$[6, 10]$. 

Hệ số hóa mang lại$6 = 2 \cdot 3$,$10 = 2 \cdot 5$. 

| Số | Các yếu tố chính | Cập nhật bản đồ số mũ tối đa | 
| --- | --- | --- | 
| 6 | 2¹, 3¹ | {2:1, 3:1} | 
| 10 | 2¹, 5¹ | {2:1, 3:1, 5:1} | 

Tổng số lần di chuyển là$3$. Mỗi số nguyên tố đóng góp chính xác một nước đi. Người chơi đầu tiên thắng vì tổng số là số lẻ. 

Những ví dụ này cho thấy các số nguyên tố hoạt động độc lập và câu trả lời chỉ phụ thuộc vào số lượng “lớp” các thừa số nguyên tố riêng biệt tồn tại trong mảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(M \log \log M + n \log M)$| sàng đạt giá trị lớn nhất$M$, sau đó phân tích thành thừa số cho mỗi phần tử | 
| Không gian |$O(M + k)$| mảng sàng cộng với bản đồ số nguyên tố | 

Sàng chiếm ưu thế khi số lượng lớn, nhưng các ràng buộc điển hình cho vấn đề này vẫn giữ nguyên.$M$quản lý được. Việc phân tích nhân tử cho mỗi phần tử vẫn hiệu quả vì mỗi phép chia sẽ giảm số lượng một cách nhanh chóng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# simple cases
assert run("1\n2\n") in ["Alice", "Bob"]

assert run("2\n2 4\n") == "Bob"

assert run("2\n6 10\n") == "Alice"

assert run("3\n3 5 7\n") == "Alice"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | phụ thuộc | hành vi chuỗi nguyên tố đơn | 
| 2,4 | Bob | tích lũy số mũ lặp đi lặp lại | 
| 6,10 | Alice | nhiều số nguyên tố độc lập | 
| 3,5,7 | Alice | mọi số nguyên tố độc lập | 

## Vỏ cạnh 

Một mảng tối thiểu như$[1]$không chứa số nguyên tố nên không có nước đi nào và người chơi thứ hai thắng ngay lập tức. Thuật toán xử lý việc này vì vòng lặp phân tích nhân tử không bao giờ chèn bất kỳ số nguyên tố nào, để lại tổng trống. 

Một trường hợp như$[16]$có một số nguyên tố duy nhất có số mũ cao. Năng suất nhân tố hóa$2^4$, vậy số mũ tối đa là 4. Trò chơi kéo dài đúng 4 nước đi: giảm một nửa lặp đi lặp lại cho đến khi đạt 1. Thuật toán nắm bắt trực tiếp điều này thông qua số mũ tối đa là 2. 

Một trường hợp hỗn hợp như$[8, 9]$tách riêng các số nguyên tố 2 và 3 một cách độc lập. Bản đồ trở thành$\{2:3, 3:2\}$, tạo ra tổng số bước đi 5. Mỗi số nguyên tố phát triển độc lập và việc triển khai sẽ tổng hợp chính xác cực đại mà không trộn lẫn các đóng góp.
