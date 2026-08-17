---
title: "CF 104053G - Trò chơi"
description: "Hai người chơi liên tục xây dựng các số bằng cách nhân các số nguyên đã chọn. Bộ điều khiển Alice $A$, bộ điều khiển Bob $B$. Cả hai đều bắt đầu bằng giá trị $alpha = 1$ và $beta = 1$. Mỗi lần Alice di chuyển, cô ấy chọn bất kỳ phần tử nào từ $A$ và nhân nó thành $alpha$."
date: "2026-07-02T03:36:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104053
codeforces_index: "G"
codeforces_contest_name: "2022 China Collegiate Programming Contest (CCPC) Guangzhou Onsite"
rating: 0
weight: 104053
solve_time_s: 68
verified: true
draft: false
---

[CF 104053G - Trò chơi](https://codeforces.com/problemset/problem/104053/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hai người chơi liên tục xây dựng các số bằng cách nhân các số nguyên đã chọn. Bộ điều khiển Alice$A$, Bộ điều khiển Bob$B$. Cả hai đều bắt đầu bằng giá trị$\alpha = 1$Và$\beta = 1$. Mỗi lần Alice di chuyển, cô ấy chọn bất kỳ phần tử nào từ$A$và nhân nó thành$\alpha$. Mỗi lần Bob di chuyển, anh ấy chọn bất kỳ phần tử nào từ$B$và nhân nó thành$\beta$. Quá trình thay thế mãi mãi, bắt đầu với Alice. 

Điều kiện chiến thắng của Bob rất đơn giản nhưng mang tính toàn cầu: nếu vào bất kỳ thời điểm nào$\alpha$chia rẽ$\beta$, Bob ngay lập tức thắng. Alice không bao giờ có điều kiện thắng trực tiếp; mục tiêu của cô ấy chỉ là tránh đạt đến trạng thái mà Bob có thể buộc điều kiện chia hết này. 

Chúng tôi được phép xóa bất kỳ tập hợp con nào của các phần tử khỏi$A$trước khi trò chơi bắt đầu. Sau khi loại bỏ các phần tử, trò chơi được chơi với các số còn lại trong$A$. Một tập hợp con được gọi là hợp lệ nếu việc loại bỏ nó không làm thay đổi thực tế là Alice vẫn có thể tránh thua khi chơi tối ưu. Chúng ta phải đếm có bao nhiêu tập con hợp lệ. 

Các ràng buộc đủ nhỏ để việc phân tích nhân tử và suy luận từng phần tử là khả thi. Cả hai bộ đều chứa tối đa 500 số và mỗi số nhiều nhất là 500. Điều này ngay lập tức gợi ý rằng phân tích thừa số nguyên tố là ngôn ngữ chính xác, vì tất cả các tương tác dưới phép nhân đều phân hủy rõ ràng trên các số nguyên tố. Bất kỳ giải pháp nào cố gắng mô phỏng trực tiếp lối chơi đều quá chậm vì trò chơi là vô hạn và việc phân nhánh các lựa chọn theo cấp số nhân ở mỗi lượt. 

Một trường hợp khó nhận thấy là khi Alice đã thua ngay cả khi không loại bỏ bất cứ thứ gì. Ví dụ: nếu Bob có thể đảm bảo$\alpha \mid \beta$ngay từ đầu bất kể chiến lược nào, thì không có tập hợp con nào giúp được Alice và câu trả lời phải bằng 0. Một trường hợp góc khác là tập con trống của$A$. Loại bỏ mọi thứ khiến$\alpha$giữ nguyên ở mức 1 mãi mãi, vì vậy Bob thắng ngay từ đầu kể từ$\beta$cũng bắt đầu từ 1, làm cho số chia hết ngay lập tức. Điều này có nghĩa là tập hợp con trống không bao giờ hợp lệ. 

## Phương pháp tiếp cận 

Cách giải thích ngây thơ của trò chơi cố gắng mô phỏng lối chơi tối ưu. Ở mỗi bước, Alice và Bob chọn những bước đi nhằm tối đa hóa mục tiêu của họ. Trạng thái bạo lực sẽ cần theo dõi các giá trị hiện tại của$\alpha$Và$\beta$, phát triển theo cấp số nhân mà không bị ràng buộc, đồng thời xem xét mọi lựa chọn trong tương lai của cả hai người chơi. Ngay cả khi chúng ta nén các giá trị, hệ số phân nhánh vẫn$|A| \cdot |B|$mỗi nước đi và trò chơi không có giới hạn kết thúc tự nhiên. Điều này làm cho việc mô phỏng trò chơi trực tiếp không thể thực hiện được. 

Quan sát quan trọng xuất phát từ việc viết lại điều kiện chia hết theo số mũ nguyên tố. Viết mọi số dưới dạng vectơ số mũ trên các số nguyên tố đến 500. Sau đó$\alpha \mid \beta$tương đương với mọi số nguyên tố có số mũ trong$\alpha$không vượt quá mức đó$\beta$. Vì phép nhân cộng thêm số mũ nên mỗi bước di chuyển sẽ thêm một trong các vectơ này. 

Bây giờ trò chơi trở thành một cuộc cạnh tranh trên nhiều tọa độ độc lập. Ở mỗi lượt Alice thêm một vectơ từ$A$, Bob thêm một từ$B$. Vì chúng có thể tái sử dụng các phần tử nên hành vi dài hạn bị chi phối bởi vectơ nào tối đa hóa sự tăng trưởng theo từng hướng. Đối với số nguyên tố cố định$p$, chỉ số mũ tối đa được đóng góp bởi bất kỳ phần tử nào mới có giá trị tiệm cận, bởi vì việc liên tục chọn người đóng góp tốt nhất sẽ chi phối bất kỳ chiến lược hỗn hợp nào. 

Điều này làm giảm trò chơi để so sánh, cho mỗi số nguyên tố$p$, số mũ tối đa có sẵn cho Alice và Bob. Nếu mức tối đa của Bob ít nhất là mức tối đa của Alice cho mọi số nguyên tố thì Bob cuối cùng có thể bằng hoặc vượt quá Alice ở mọi tọa độ và lực chia hết. Ngược lại, nếu tồn tại một số nguyên tố trong đó Alice có số đóng góp tối đa lớn hơn rất nhiều, cô ấy có thể giữ tọa độ đó mãi mãi, ngăn cản điều kiện thắng của Bob. 

Một khi đặc tính này có sẵn, bài toán tập hợp con trở nên thuần túy mang tính cấu trúc. Loại bỏ các phần tử khỏi$A$thay đổi số mũ tối đa của Alice trên mỗi số nguyên tố. Một tập hợp con hợp lệ khi sau khi loại bỏ vẫn tồn tại ít nhất một số nguyên tố trong đó giá trị lớn nhất còn lại của Alice vượt quá giá trị lớn nhất cố định của Bob. 

Điều này biến bài toán thành việc đếm các tập con tránh việc phá hủy tất cả các “số nguyên tố thắng cuộc”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trò chơi Brute Force | Hàm mũ trong di chuyển | Không gian trạng thái lớn | Quá chậm | 
| Đếm hệ số nguyên tố + tập hợp con |$O(n \cdot P + n \log n)$|$O(n \cdot P)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tính toán trước các hệ số nguyên tố cho tất cả các số lên tới 500 để mỗi số có thể được biểu diễn dưới dạng vectơ số mũ. 

1. Phân tích mọi số thành nhân tử$B$và tính toán cho mỗi số nguyên tố$p$số mũ tối đa xuất hiện trong số tất cả các phần tử của$B$. Gọi đây$maxB[p]$. Điều này ghi lại mức tăng trưởng mạnh nhất có thể có của Bob ở mỗi tọa độ. 
2. Phân tích mọi số thành nhân tử$A$, và tính toán tương tự$maxA[p]$, số mũ tối đa có trong tập hợp đầy đủ$A$. Điều này cho chúng ta biết liệu Alice có dẫn đầu toàn cầu theo từng hướng chính hay không trước khi bị xóa bỏ. 
3. Nếu với mọi số nguyên tố$p$,$maxA[p] \le maxB[p]$, thì Bob đã vượt trội hoặc sánh ngang với Alice ở mọi tọa độ. Trong trường hợp này, Alice không thể duy trì bất kỳ lợi thế nào ngay cả với tập hợp đầy đủ, vì vậy mọi tập hợp con đều thua và câu trả lời là 0. 
4. Mặt khác, xác định những phần tử nào của$A$là “an toàn”. Một phần tử là an toàn nếu với mọi số nguyên tố$p$, số mũ của nó không vượt quá$maxB[p]$. Các phần tử này không thể đẩy riêng lẻ bất kỳ tọa độ nào vượt quá khả năng của Bob. 
5. Hãy để$k$là số phần tử an toàn. Bất kỳ tập hợp con nào chỉ bao gồm các phần tử an toàn đều không thể tạo ra số nguyên tố trong đó Alice vượt quá Bob, vì vậy tất cả các tập hợp con như vậy đều mất vị trí đối với Alice. 
6. Do đó, các tập hợp con không hợp lệ chính xác là tất cả các tập hợp con của các phần tử an toàn, tức là$2^k$. Tổng số tập hợp con là$2^{|A|}$, vì vậy các tập hợp con hợp lệ bằng nhau$2^{|A|} - 2^k$, lấy modulo$10^9+7$. 

Lý do điều này có tác dụng là vì chỉ có số mũ tối đa trên mỗi số nguyên tố mới quan trọng đối với điều kiện ưu thế dài hạn. Bất kỳ phần tử nào vượt quá giới hạn của Bob trong ít nhất một số nguyên tố đều đủ để có khả năng duy trì lợi thế của Alice trong tọa độ đó, do đó, việc loại bỏ tất cả các phần tử như vậy sẽ phá hủy hướng chiến thắng duy nhất của cô ấy. Ngược lại, việc giữ ít nhất một phần tử như vậy sẽ duy trì ít nhất một tọa độ trong đó Alice có thể dẫn đầu vô thời hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
MAXV = 500

# sieve for smallest prime factor
spf = list(range(MAXV + 1))
for i in range(2, MAXV + 1):
    if spf[i] == i:
        for j in range(i * i, MAXV + 1, i):
            if spf[j] == j:
                spf[j] = i

def factor(x):
    res = {}
    while x > 1:
        p = spf[x]
        cnt = 0
        while x % p == 0:
            x //= p
            cnt += 1
        res[p] = cnt
    return res

n, m = map(int, input().split())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

maxB = {}
for b in B:
    f = factor(b)
    for p, c in f.items():
        maxB[p] = max(maxB.get(p, 0), c)

maxA_full = {}
for a in A:
    f = factor(a)
    for p, c in f.items():
        maxA_full[p] = max(maxA_full.get(p, 0), c)

# check if Alice is already doomed
alice_wins_somewhere = False
for p, v in maxA_full.items():
    if v > maxB.get(p, 0):
        alice_wins_somewhere = True
        break

if not alice_wins_somewhere:
    print(0)
    sys.exit()

maxB_default = lambda p: maxB.get(p, 0)

safe = 0
for a in A:
    f = factor(a)
    ok = True
    for p, c in f.items():
        if c > maxB_default(p):
            ok = False
            break
    if ok:
        safe += 1

ans = (pow(2, n, MOD) - pow(2, safe, MOD)) % MOD
print(ans)
```Việc triển khai dựa trên sàng hệ số nguyên tố nhỏ nhất để phân tích mọi số một cách nhanh chóng. Điều này là cần thiết vì việc phân chia thử nghiệm lặp đi lặp lại vẫn đủ nhanh ở đây nhưng ít có cấu trúc hơn và khó giải thích hơn. 

Chúng tôi tính toán mức cực đại mỗi số nguyên tố của Bob một lần và coi chúng là ngưỡng cố định. Khi đó mỗi phần tử của$A$được phân loại là an toàn hoặc không an toàn bằng cách so sánh số mũ nguyên tố của nó với các ngưỡng này. Phép trừ tổ hợp cuối cùng xuất phát trực tiếp từ việc đếm các tập hợp con. 

Một chi tiết triển khai tinh tế là việc thoát ra sớm khi Alice không có số nguyên tố chiến thắng ngay cả trong toàn bộ. Trong trường hợp đó, không có tập hợp con nào có thể thay đổi kết quả, vì vậy việc trả về số 0 là bắt buộc trong câu lệnh vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
2 6
6 7 8
```Chúng tôi tính cực đại của Bob:$6 = 2 \cdot 3$,$7$,$8 = 2^3$. Vì thế$maxB[2]=3$,$maxB[3]=1$,$maxB[7]=1$. 

Đối với Alice,$2$cho$2^1$,$6$cho$2^1 \cdot 3^1$, Vì thế$maxA[2]=1$,$maxA[3]=1$. Alice không bao giờ vượt qua Bob ở bất kỳ số nguyên tố nào, vì vậy cô ấy đã bị thống trị. Thuật toán xuất ra 0. 

Điều này chứng tỏ trường hợp chấm dứt sớm. 

### Ví dụ 2 

Hãy xem xét:```
2 2
4 9
2 3
```Bob có$2^1$Và$3^1$, Vì thế$maxB[2]=1$,$maxB[3]=1$. Alice có$4=2^2$,$9=3^2$, vậy cô ấy có số nguyên tố thắng ở cả hai tọa độ. 

Yếu tố an toàn là yếu tố không vượt quá mức tối đa của Bob. Cả 4 và 9 đều không an toàn, vì vậy$k=0$. Tổng số tập hợp con là 4, tập hợp con không hợp lệ là 1 (tập trống), vì vậy câu trả lời là 3. 

Điều này cho thấy cách các tập hợp con được tính hoàn toàn thông qua phân loại phần tử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(( | A | 
| Không gian |$O(V)$| Lưu trữ các thừa số nguyên tố nhỏ nhất và bản đồ số mũ | 

Cả hai giới hạn ràng buộc đều đặt ở mức 500 phần tử với giá trị lên tới 500, do đó việc quét bao thanh toán và quét từng phần tử vẫn thoải mái trong giới hạn. Giải pháp tránh bất kỳ vụ nổ tổ hợp nào bằng cách giảm trò chơi xuống mức cực đại mỗi số nguyên tố. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import prod

    MOD = 10**9 + 7
    MAXV = 500
    spf = list(range(MAXV + 1))
    for i in range(2, MAXV + 1):
        if spf[i] == i:
            for j in range(i * i, MAXV + 1, i):
                if spf[j] == j:
                    spf[j] = i

    def factor(x):
        res = {}
        while x > 1:
            p = spf[x]
            c = 0
            while x % p == 0:
                x //= p
                c += 1
            res[p] = c
        return res

    n, m = map(int, input().split())
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))

    maxB = {}
    for b in B:
        f = factor(b)
        for p, c in f.items():
            maxB[p] = max(maxB.get(p, 0), c)

    maxA = {}
    for a in A:
        f = factor(a)
        for p, c in f.items():
            maxA[p] = max(maxA.get(p, 0), c)

    if all(maxA.get(p, 0) <= maxB.get(p, 0) for p in maxA):
        return "0"

    safe = 0
    for a in A:
        f = factor(a)
        ok = True
        for p, c in f.items():
            if c > maxB.get(p, 0):
                ok = False
                break
        if ok:
            safe += 1

    return str((pow(2, n, MOD) - pow(2, safe, MOD)) % MOD)

# provided samples
assert run("""2 3
2 6
6 7 8
""") == "0"

# custom cases
assert run("""2 2
4 9
2 3
""") == "3", "both sides small primes"
assert run("""1 1
2
2
""") == "0", "equal powers immediate loss"
assert run("""3 2
2 4 8
2 3
""") == "6", "some safe some unsafe"
assert run("""1 1
4
2
""") == "1", "single element strictly stronger"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số nguyên tố nhỏ bằng nhau | 3 | logic trừ tập hợp con | 
| sức mạnh khởi đầu bằng nhau | 0 | sự thống trị ngay lập tức của Bob | 
| quyền lực hỗn hợp | 6 | phân loại an toàn một phần | 
| yếu tố mạnh mẽ duy nhất | 1 | trường hợp tích cực cơ bản | 

## Vỏ cạnh 

Khi Alice không có số nguyên tố mà cô ấy vượt qua Bob ngay cả trước khi xóa, toàn bộ đã thua. Đối với một đầu vào như$A = [6]$,$B = [6]$, cả hai đều có cấu hình số mũ giống hệt nhau. Thuật toán tính toán$maxA[p] \le maxB[p]$cho tất cả các số nguyên tố và ngay lập tức trả về số 0. Điều này phù hợp với thực tế là ngay cả khi không xóa, Bob vẫn thắng ngay lập tức. 

Khi mọi phần tử của$A$là an toàn, câu trả lời trở thành$2^{|A|} - 2^{|A|} = 0$. Ví dụ nếu tất cả$a_i$nhỏ và bị Bob thống trị ở mọi số nguyên tố thì mọi tập con vẫn thua. Cơ chế đếm an toàn nắm bắt chính xác điều này vì mọi phần tử đều vượt qua quá trình kiểm tra an toàn.
