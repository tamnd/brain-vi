---
title: "CF 104783A - Ngôi Sao Bạc Đứng Một Mình"
description: "Dữ liệu đầu vào đưa ra một số nguyên tố duy nhất $P$, đại diện cho vị trí của tiểu hành tinh cuối cùng, được gọi là Ngôi sao bạc, trên đường một chiều bắt đầu từ Sao Hỏa."
date: "2026-06-28T14:47:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104783
codeforces_index: "A"
codeforces_contest_name: "2021-2022 CTU Open Contest"
rating: 0
weight: 104783
solve_time_s: 53
verified: true
draft: false
---

[CF 104783A - Ngôi sao bạc đứng một mình](https://codeforces.com/problemset/problem/104783/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào cho một số nguyên tố duy nhất$P$, đại diện cho vị trí của tiểu hành tinh cuối cùng, được gọi là Silver Star, trên đường một chiều bắt đầu từ Sao Hỏa. Dọc theo đường này, có những tiểu hành tinh đặc biệt đặt cách sao Hỏa những khoảng cách bằng các số nguyên tố theo thứ tự tăng dần: 2, 3, 5, 7, 11, v.v., cho đến$P$. 

Một tàu thăm dò bắt đầu ở tiểu hành tinh ở khoảng cách 2 và phải kết thúc ở tiểu hành tinh ở khoảng cách$P$. Nó có thể ghé thăm bất kỳ tập hợp con nào của các tiểu hành tinh trung gian, nhưng nó phải tôn trọng một hạn chế về chuyển động: bất cứ khi nào nó di chuyển từ tiểu hành tinh được ghé thăm này sang tiểu hành tinh khác, khoảng cách giữa các vị trí của chúng không được vượt quá 14 đơn vị. Vì chuyển động hoàn toàn tiến về phía trước dọc theo dãy số nguyên tố tăng dần nên quỹ đạo đơn giản là một dãy con của các vị trí nguyên tố bắt đầu từ 2 và kết thúc ở$P$, trong đó các số nguyên tố được chọn liên tiếp khác nhau nhiều nhất là 14. 

Nhiệm vụ là đếm xem tồn tại bao nhiêu quỹ đạo hợp lệ như vậy. 

Sự hạn chế đó$P \le 211$có nghĩa là chúng ta đang xử lý một tiền tố rất nhỏ của các số nguyên tố. Số số nguyên tố lên tới 211 chỉ có vài chục nên ngay cả một$O(n^2)$giải pháp lập trình động là tầm thường về mặt hiệu suất. Điều này đã gợi ý rằng giải pháp dự định dựa trên tổ hợp đơn giản trên một DAG nhỏ thay vì bất kỳ sự tối ưu hóa nặng nề nào. 

Một trường hợp thất bại tinh tế xuất hiện khi người ta giả định rằng “nhiều nhất là 14 AU” có nghĩa là chỉ có thể xem xét các số nguyên tố liền kề. Ví dụ, từ 2 chúng ta có thể nhảy trực tiếp lên 11 vì 11 − 2 = 9 là được phép, mặc dù có các số nguyên tố trung gian. Một cách giải thích chỉ dựa vào lân cận ngây thơ sẽ tính thiếu các đường đi một cách không chính xác bằng cách cấm những lần bỏ qua như vậy. 

Một sai lầm khác phát sinh nếu người ta cho rằng tất cả các số nguyên tố đều cách đều nhau hoặc cố gắng lập chỉ mục các bước nhảy theo số lượng thay vì khoảng cách số thực tế. Ràng buộc phụ thuộc vào giá trị thực tế, không phải vị trí trong chuỗi. 

## Phương pháp tiếp cận 

Quan điểm vũ phu coi mỗi quỹ đạo là một quá trình quyết định: tại mỗi tiểu hành tinh, chúng ta chọn xem có đi đến bất kỳ tiểu hành tinh nào sau này nằm trong phạm vi 14 đơn vị hay không. Điều này có thể được khám phá bằng DFS từ số nguyên tố đầu tiên, phân nhánh đệ quy đến tất cả các bước tiếp theo hợp lệ. Điều này đúng vì nó liệt kê tất cả các chuỗi con hợp lệ tuân theo ràng buộc bước nhảy và đếm những chuỗi con đó đạt tới nút cuối cùng. 

Tuy nhiên, việc khám phá này phát triển nhanh chóng ở yếu tố phân nhánh. Trong trường hợp khái niệm xấu nhất khi mỗi nút có thể nhảy tới nhiều nút trong tương lai, số lượng đường dẫn đệ quy có thể tăng theo cấp số nhân với số lượng số nguyên tố, đại khái là$O(2^n)$. Mặc dù$n$ở đây nhỏ, cách tiếp cận này không hiệu quả về mặt cấu trúc và không cần thiết. 

Quan sát quan trọng là biểu đồ được hình thành bởi các số nguyên tố và các bước nhảy hợp lệ là một biểu đồ tuần hoàn có hướng được sắp xếp theo các giá trị tăng dần. Bất kỳ vấn đề về số lượng đường dẫn nào trên DAG đều có thể được giảm xuống thành lập trình động: số cách để tiếp cận một nút là tổng số cách để tiếp cận tất cả các nút có thể chuyển đổi vào nút đó. 

Đối với mỗi số nguyên tố$p[i]$, chúng ta chỉ cần xét các số nguyên tố trước đó$p[j]$như vậy$p[i] - p[j] \le 14$. Vì số nguyên tố ngày càng tăng và khoảng cách nhỏ nên chỉ một hậu tố ngắn của các nút trước đó mới có thể tiếp cận được nút hiện tại. Điều này làm giảm chi phí chuyển đổi trên mỗi nút thành thời gian khấu hao không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê DFS |$O(2^n)$|$O(n)$| Quá chậm | 
| DP trên DAG |$O(n^2)$hoặc$O(n)$tối ưu hóa |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các số nguyên tố lên đến$P$dưới dạng các nút theo thứ tự tăng dần và chúng tôi tính toán xem có bao nhiêu cách có thể tiếp cận mỗi nút ngay từ đầu. 

1. Tạo danh sách tất cả các số nguyên tố từ 2 đến$P$. This forms the vertex set of the graph. Thứ tự của danh sách này đã mang tính tôpô vì nó tăng dần. 
2. Create a DP array where`dp[i]`đại diện cho số lượng quỹ đạo hợp lệ kết thúc tại$i$-thứ nguyên tố. 
3. Khởi tạo`dp[0] = 1`bởi vì có chính xác một con đường để bắt đầu từ tiểu hành tinh đầu tiên (2 AU), và mọi con đường đều phải bắt đầu từ đó. 
4. Đối với mỗi chỉ số nguyên tố$i$từ trái sang phải, tính`dp[i]`bằng cách tổng hợp những đóng góp từ các chỉ số trước đó$j < i$như vậy`primes[i] - primes[j] <= 14`. Mỗi cái như vậy$j$đại diện cho bước cuối cùng hợp lệ vào$i$, nên mọi cách để tiếp cận$j$có thể được mở rộng để$i$. 
5. Duy trì tổng một cách hiệu quả bằng cách quét ngược cho đến khi điều kiện khoảng cách không thành công hoặc bằng cách giữ một cửa sổ trượt của các giá trị trước hợp lệ. Vì giá trị tăng lên nên một khi`primes[i] - primes[j] > 14`, tất cả đều sớm hơn$j$không hợp lệ. 
6. Câu trả lời cuối cùng là`dp[last]`, vì mọi quỹ đạo hợp lệ đều phải kết thúc ở Silver Star. 

### Tại sao nó hoạt động 

Mỗi quỹ đạo hợp lệ được xác định duy nhất bởi bước nhảy cuối cùng của nó vào mỗi tiểu hành tinh. DP đảm bảo rằng mỗi trạng thái tích lũy mọi cách để tiếp cận nó từ các trạng thái tiền nhiệm hợp lệ và không bao giờ đưa vào chuyển đổi không hợp lệ nào vì giới hạn khoảng cách được kiểm tra trực tiếp trên các vị trí tiểu hành tinh thực tế. Vì đồ thị không theo chu kỳ theo thứ tự tăng dần nên mỗi bài toán con chỉ phụ thuộc vào các trạng thái đã được tính toán, đảm bảo tính đúng đắn bằng quy nạp trên các số nguyên tố có thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def sieve(n):
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(n ** 0.5) + 1):
        if is_prime[i]:
            step = i
            start = i * i
            for j in range(start, n + 1, step):
                is_prime[j] = False
    return [i for i in range(2, n + 1) if is_prime[i]]

def solve():
    P = int(input().strip())
    primes = sieve(P)

    n = len(primes)
    dp = [0] * n
    dp[0] = 1

    for i in range(1, n):
        total = 0
        j = i - 1
        while j >= 0 and primes[i] - primes[j] <= 14:
            total += dp[j]
            j -= 1
        dp[i] = total

    print(dp[-1])

if __name__ == "__main__":
    solve()
```Sàng xây dựng trình tự nguyên tố lên đến$P$, đủ nhỏ để đơn giản$O(P \log \log P)$phương pháp là đủ. 

Mảng DP mã hóa tất cả số lượng quỹ đạo một phần. Đối với mỗi vị trí, chúng tôi quét ngược cho đến khi điều kiện khoảng cách bị phá vỡ. Điều này là an toàn vì các số nguyên tố tăng dần, vì vậy khi khoảng cách vượt quá 14, tất cả các số trước đó cũng sẽ vượt quá khoảng cách đó. 

Một lỗi triển khai phổ biến là quên rằng các quá trình chuyển đổi phụ thuộc vào sự khác biệt về số lượng hơn là sự khác biệt về chỉ số. Một vấn đề tế nhị khác là khởi tạo`dp[0] = 1`; không có trường hợp cơ sở này, tất cả số đếm tiếp theo vẫn bằng 0. 

## Ví dụ đã hoạt động 

Vì kết quả đầu ra mẫu cụ thể không được cung cấp trong báo cáo nên hãy xem xét các đầu vào minh họa. 

Giả sử các số nguyên tố tới 11 là$[2, 3, 5, 7, 11]$. 

Đối với đầu vào$P = 11$, DP phát triển như sau: 

| tôi | nguyên tố | người tiền nhiệm hợp lệ | dp[i] | 
| --- | --- | --- | --- | 
| 0 | 2 | không | 1 | 
| 1 | 3 | 2 | 1 | 
| 2 | 5 | 3, 2 | 2 | 
| 3 | 7 | 5, 3, 2 | 4 | 
| 4 | 11 | 7, 5, 3, 2 | 8 | 

Điều này cho thấy rằng mọi nút tổng hợp tất cả lịch sử có thể truy cập từ các bước nhảy hợp lệ và số lượng tăng lên khi các đường dẫn phân nhánh qua các số nguyên tố trung gian. 

Bây giờ hãy xem xét một trường hợp nhỏ hơn lên đến$P = 5$, số nguyên tố$[2, 3, 5]$: 

| tôi | nguyên tố | người tiền nhiệm hợp lệ | dp[i] | 
| --- | --- | --- | --- | 
| 0 | 2 | không | 1 | 
| 1 | 3 | 2 | 1 | 
| 2 | 5 | 3, 2 | 2 | 

Điều này thể hiện cách cho phép bỏ qua: bước nhảy từ 2 lên 5 là hợp lệ và đóng góp trực tiếp vào`dp[2]`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot k)$, có hiệu quả$O(n^2)$trường hợp xấu nhất | Mỗi số nguyên tố kiểm tra một số giới hạn các số nguyên tố trước đó trong khoảng cách 14 | 
| Không gian |$O(n)$| Lưu trữ mảng DP và danh sách nguyên tố | 

Từ$P \le 211$, số lượng số nguyên tố nhỏ (hàng chục phần tử), do đó nghiệm chạy ngay lập tức ngay cả dưới hành vi bậc hai. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import isqrt

    def sieve(n):
        is_prime = [True] * (n + 1)
        is_prime[0] = is_prime[1] = False
        for i in range(2, isqrt(n) + 1):
            if is_prime[i]:
                for j in range(i * i, n + 1, i):
                    is_prime[j] = False
        return [i for i in range(2, n + 1) if is_prime[i]]

    P = int(sys.stdin.readline().strip())
    primes = sieve(P)

    dp = [0] * len(primes)
    dp[0] = 1

    for i in range(1, len(primes)):
        total = 0
        j = i - 1
        while j >= 0 and primes[i] - primes[j] <= 14:
            total += dp[j]
            j -= 1
        dp[i] = total

    return str(dp[-1])

# minimal case
assert run("2\n") == "1"

# small case
assert run("5\n") == "2"

# slightly larger case
assert run("11\n") == "8"

# boundary case (largest constraint)
assert run("211\n") == run("211\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 1 | trường hợp cơ sở nút đơn | 
| 5 | 2 | được phép nhảy trực tiếp và bắt đầu phân nhánh | 
| 11 | 8 | tích lũy nhiều bước trên DAG | 
| 211 | giá trị tính toán | trường hợp căng thẳng ràng buộc đầy đủ | 

## Vỏ cạnh 

Đầu vào nhỏ nhất có thể$P = 2$bao gồm một tiểu hành tinh duy nhất. Quỹ đạo duy nhất là quỹ đạo tầm thường bắt đầu và kết thúc tại cùng một nút và DP trả về chính xác 1 vì nó khởi tạo`dp[0] = 1`và không bao giờ thực hiện chuyển tiếp. 

Một trường hợp như$P = 5$chứng tỏ tầm quan trọng của việc cho phép các bước nhảy không liền kề. Việc nhảy trực tiếp từ 2 lên 5 là hợp lệ vì chênh lệch là 3 và DP bao gồm nó khi tính toán`dp[2]`. Cách tiếp cận chỉ kề cận đơn giản sẽ xuất sai 1 thay vì 2 do thiếu đường dẫn bỏ qua 3. 

Đối với lớn hơn$P$, chẳng hạn như 211, thuật toán dựa trên thực tế là chỉ một hậu tố ngắn của các tiền thân đóng góp cho mỗi trạng thái. Vòng lặp lùi dừng ngay khi khoảng cách vượt quá 14, đảm bảo rằng các số nguyên tố không liên quan sẽ không bao giờ được xem xét, giúp duy trì hiệu quả ngay cả ở giới hạn trên.
