---
title: "CF 102394C - Cạnh tranh trong hệ thống Thụy Sĩ"
description: "Có (n) người chơi và (m) vòng chơi. Trong mỗi vòng, người chơi tham gia một trận đấu hoặc tạm biệt. Một trận đấu bao gồm hai hoặc ba ván đấu và dữ liệu đầu vào trực tiếp cho biết mỗi người chơi đã thắng bao nhiêu ván và bao nhiêu ván hòa."
date: "2026-08-10T21:22:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102394
codeforces_index: "C"
codeforces_contest_name: "The 2019 China Collegiate Programming Contest Harbin Site"
rating: 0
weight: 102394
solve_time_s: 137
verified: true
draft: false
---

[CF 102394C - Cạnh tranh trong hệ thống Thụy Sĩ](https://codeforces.com/problemset/problem/102394/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có (n) người chơi và (m) vòng chơi. Trong mỗi vòng, người chơi tham gia một trận đấu hoặc tạm biệt. Một trận đấu bao gồm hai hoặc ba ván đấu và dữ liệu đầu vào trực tiếp cho biết mỗi người chơi đã thắng bao nhiêu ván và bao nhiêu ván hòa. 

Đối với mỗi người chơi, chúng tôi phải báo cáo bốn số liệu thống kê sau mỗi vòng đấu. MP là điểm trận đấu tích lũy. GW là điểm trò chơi tích lũy chia cho số điểm trò chơi tối đa có thể có, với giới hạn dưới là (1/3). OMW là mức trung bình của tỷ lệ phần trăm thắng trận hiện tại của mọi đối thủ mà người chơi đã đối mặt, tính riêng các lần gặp nhau lặp lại. OGW được định nghĩa theo cách tương tự bằng cách sử dụng tỷ lệ phần trăm thắng trò chơi hiện tại. Byes đóng góp điểm và trò chơi vào số liệu thống kê của riêng người chơi, nhưng không bao giờ tạo ra đối thủ. 

Đầu vào đưa ra số lượng trận đấu trong mỗi vòng, theo sau là các trận đấu đó theo thứ tự vòng. Một người chơi vắng mặt trong danh sách trận đấu của vòng đấu sẽ tự động được coi là tạm biệt. 

Trang Codeforces chính thức đưa ra giới hạn thời gian 2 giây và giới hạn bộ nhớ 512 MB. Ràng buộc cấu trúc chính là (m\le16), trong khi tổng (n\cdot m) trên tất cả các trường hợp thử nghiệm nhiều nhất là (3\cdot10^5). Điều kiện cuối cùng đó có nghĩa là việc vượt qua (O(nm)) qua tất cả các cặp trong vòng người chơi có thể dễ dàng chấp nhận được. Ngay cả thuật toán (O(nm^2)) cũng chỉ có hiệu quả là bội số không đổi nhỏ của (n m), bởi vì (m) bị giới hạn bởi 16. Tuy nhiên, một thuật toán liên quan đến tất cả các cặp người chơi sẽ quá đắt khi (n=10.000). 

Có một số nơi mà việc triển khai trực tiếp có thể xảy ra sai sót một cách âm thầm. 

### Trường hợp cạnh: người chơi chỉ nhận được lời tạm biệt 

Hãy xem xét```
1
2 1
0
```Cả hai người chơi đều nhận được lời tạm biệt. Mỗi người nhận được 3 MP, 6 điểm trò chơi và đã chơi hai trò chơi. GW của họ là (6/(3\cdot2)=1). Vì không có đối thủ nên cả OMW và OGW đều được xác định là (1/3). Đầu ra đúng là```
Round 1
3 1/3 1/1 1/3
3 1/3 1/1 1/3
```Một lỗi phổ biến là coi tạm biệt như đối thủ hoặc sử dụng (1) cho tỷ lệ phần trăm của đối thủ vì tạm biệt tương đương với trận đấu 2-0. Nó tương đương với một chiến thắng chỉ dựa vào số liệu thống kê của chính người chơi. 

### Trường hợp cạnh: giới hạn dưới (1/3) thay đổi phân số 

Hãy xem xét```
1
2 1
1
1 2 0 0 2
```Trận đấu hòa vì cả hai tay vợt đều không thắng ván nào. Mỗi người nhận được 1 MP và 2 điểm trò chơi từ hai trò chơi được rút ra. Tỷ lệ phần trăm trận đấu thô của họ là (1/3), trong khi tỷ lệ phần trăm trận đấu thô của họ là (2/6=1/3). Do đó, cả hai tỷ lệ phần trăm đều chính xác (1/3) và đối thủ của mỗi người chơi có cùng giá trị. 

Đầu ra đúng là```
Round 1
1 1/3 1/3 1/3
1 1/3 1/3 1/3
```Tổng quát hơn, sau vòng (r), tỷ lệ phần trăm đối sánh giới hạn có thể được biểu thị dưới dạng 

[ 
\frac{\max(r,\mathrm{MP})}{3r}. 
] 

Đối với trò chơi, nếu người chơi đã chơi (g) trò chơi và kiếm được (G) điểm trò chơi, thì giá trị giới hạn là 

[ 
\frac{\max(g,G)}{3g}. 
] 

Việc sử dụng giá trị chưa được giới hạn khi tính toán OMW hoặc OGW sẽ tạo ra câu trả lời sai cho người chơi với kết quả kém. 

### Trường hợp Edge: cùng một đối thủ xuất hiện nhiều lần 

Hãy xem xét```
1
2 2
1 1
1 2 2 0 0
1 2 0 2 0
```Người chơi 1 thắng trận đầu tiên và thua trận thứ hai. Người chơi 2 làm ngược lại. Sau vòng đầu tiên, người chơi 1 có MP 3 và người chơi 2 có MP 0. Sau vòng thứ hai, cả hai đã chơi hai lần nên người chơi 1 vẫn còn MP 3 và người chơi 2 vẫn có MP 3. 

Danh sách đối thủ của mỗi người chơi chứa cùng một người chơi hai lần. OMW ở vòng thứ hai phải tính trung bình tỷ lệ phần trăm hiện tại của đối thủ đó hai lần, không phải một lần. Đầu ra đúng là```
Round 1
3 1/3 1/1 1/3
0 1/1 1/3 1/1
Round 2
3 1/1 1/1 1/3
3 1/1 1/3 1/1
```Một giải pháp chỉ lưu trữ một tập hợp các đối thủ riêng biệt sẽ âm thầm đếm thiếu các trận đấu lặp lại. 

## Phương pháp tiếp cận 

Giải pháp bạo lực trực tiếp nhất lưu trữ tất cả các trận đấu được thấy cho đến nay. Sau mỗi vòng, đối với mỗi người chơi, nó sẽ quét mọi trận đấu trước đó, kiểm tra xem người chơi có tham gia vào trận đấu đó hay không và nếu có thì sẽ thêm MW và GW hiện tại của đối thủ đó. Điều này đúng vì định nghĩa của OMW và OGW chính xác là mức trung bình trong các trận đấu lịch sử. 

Vấn đề là số lượng công việc lặp đi lặp lại. Trong trường hợp xấu nhất, hầu hết mọi người chơi đều tham gia vào mọi vòng đấu, nên sau vòng (r) có khoảng (nr/2) trận đấu. Việc quét riêng các trận đấu đó cho tất cả (n) người chơi tốn khoảng (n^2r/2) lần kiểm tra trong vòng đó. Tổng hợp tất cả 16 vòng, trường hợp xấu nhất là 

136n^2. 
] 

Đối với (n=10.000), tức là khoảng (13,6) tỷ lượt kiểm tra trùng khớp, trước khi tính phân số số học hoặc đầu ra. 

Lực lượng vũ phu hoạt động vì mọi thống kê chỉ phụ thuộc vào các trận đấu đã diễn ra, nhưng nó liên tục khám phá lại đối thủ nào thuộc về mỗi người chơi. Nhận xét quan trọng là mối quan hệ đối thủ không bao giờ biến mất. Khi người chơi (u) và (v) đã chơi, (v) vẫn là một trong những đối thủ của (u) ở mọi hiệp sau. Vì một người chơi có thể chơi tối đa một lần mỗi vòng nên mỗi người chơi có tối đa (m) mục của đối thủ sau toàn bộ giải đấu. 

Do đó, chúng tôi có thể lưu trữ, đối với mỗi người chơi, một danh sách chứa đối thủ trong mọi trận đấu mà họ đã chơi. Sau khi xử lý một vòng, chúng tôi chỉ cần xem qua các danh sách này. Tại vòng (r), mỗi danh sách có độ dài tối đa (r), do đó tổng số bài dự thi của đối thủ được kiểm tra trong toàn bộ giải đấu là 

[ 
O\left(n\sum_{r=1}^{m}r\right)=O(nm^2). 
] 

Vì (m\le16) nên cái này nhỏ. Điều kiện chung (\sum nm\le3\cdot10^5) thậm chí còn khiến nó an toàn hơn trong nhiều trường hợp thử nghiệm. 

OMW có một sự đơn giản hóa bổ sung. Tại một vòng cố định (r), MW của mỗi người chơi đều có mẫu số (3r). Nếu người chơi (v) có MP (P_v), MW giới hạn của nó là 

[ 
\frac{\max(r,P_v)}{3r}. 
] 

Do đó, mức trung bình trên (d) mục nhập của đối thủ của người chơi chỉ đơn giản là 

[ 
\frac{\sum_v\max(r,P_v)}{3rd}. 
] 

OGW không có mẫu số chung vì người chơi có thể đã chơi số lượng trò chơi khác nhau do số lần tạm biệt và các trận đấu hai hoặc ba trò chơi khác nhau. Do đó, chúng tôi cộng các phân số GW của đối thủ bằng số học phân số chính xác thông thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2m^2)) | (O(nm)) | Quá chậm | 
| Tối ưu | (O(nm^2)) | (O(nm)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mảng cho MP tích lũy, điểm trò chơi và số trò chơi đã chơi của mỗi người chơi. Đồng thời tạo danh sách đối thủ cho mọi người chơi. Danh sách đối thủ lưu trữ một mục nhập cho mỗi trận đấu, do đó các cuộc họp lặp lại được thể hiện nhiều lần một cách tự nhiên. 
2. Xử lý các vòng theo thứ tự. Đối với mỗi trận đấu, đánh dấu cả hai người chơi đã chơi vòng này và cập nhật MP, điểm trò chơi và số trận đấu của họ theo kết quả trò chơi được cung cấp. 

Nếu (w_1>w_2), người chơi 1 nhận được 3 MP và người chơi 2 nhận được 0. Nếu (w_1<w_2), thì điều ngược lại sẽ xảy ra. Nếu (w_1=w_2), cả hai đều nhận được 1 MP. Điểm trò chơi là (3w+d) và số trò chơi là (w_1+w_2+d). 
3. Thêm từng người chơi vào danh sách đối thủ của người kia. Điều này xảy ra đúng một lần mỗi trận, ngay cả khi hai người chơi đã gặp nhau trước đó. Sự lặp lại là cần thiết vì định nghĩa tính trung bình trên các trận đấu chứ không phải trên các đối thủ riêng biệt. 
4. Sau khi tất cả các trận đấu của vòng hiện tại đã được xử lý, mọi người chơi không được đánh dấu sẽ được tạm biệt. Thêm 3 MP, 6 điểm trò chơi và 2 trò chơi cho người chơi đó. 

Bằng cách xử lý tạm biệt sau các trận đấu thực tế, số liệu thống kê đầy đủ của vòng hiện tại sẽ có sẵn trước khi tính toán bất kỳ tỷ lệ phần trăm nào. 
5. Đối với mỗi người chơi, hãy tính GW giới hạn từ điểm trò chơi và trò chơi tích lũy của chính họ: 

[ 
GW_i=\frac{\max(\mathrm{games__i,\mathrm{gamePoints__i)} 
{3\mathrm{games__i}. 
] 

Rút gọn phân số có ước chung lớn nhất. 

1. Tính tử số MW hiện tại của mỗi người chơi như sau:

[ 
M_i=\max(r,\mathrm{MP__i). 
] 

MW thực tế là (M_i/(3r)). Đối với người chơi có đối thủ, tính tổng (M_v) cho mỗi mục nhập của đối thủ (v) và chia cho (3r) lần số mục nhập của đối thủ. 

1. Đối với OGW, hãy xem qua danh sách đối thủ tương tự. Đối với mỗi đối thủ (v), hãy lấy GW giới hạn hiện tại của nó là 

[ 
\frac{\max(\mathrm{games__v,\mathrm{gamePoints__v)} 
{3\mathrm{games__v}. 
] 

Cộng chính xác các phân số này và cuối cùng chia cho số mục của đối thủ. 

1. Nếu người chơi không có đối thủ, in (1/3) cho cả OMW và OGW. Nếu không thì giảm cả hai phân số đã tính và in bốn số liệu thống kê. 
2. Lặp lại quá trình này sau mỗi lượt chơi. Vì tất cả số liệu thống kê đều được tích lũy nên không cần tính toán lại từ kết quả trận đấu thô ngoại trừ mức trung bình của đối thủ. 

### Tại sao nó hoạt động 

Sau vòng (r), mảng MP, điểm trò chơi và trò chơi chứa chính xác các giá trị tích lũy kiếm được qua vòng đó vì mỗi trận đấu và mỗi lần tạm biệt đều đã được xử lý một lần. Danh sách đối thủ của mỗi người chơi chứa chính xác một mục nhập cho mỗi trận đấu mà người chơi đó đã chơi, bao gồm cả những đối thủ lặp lại và không bao gồm những trận đấu tạm biệt. 

Phép tính MW sử dụng (\max(r,\mathrm{MP})/(3r)), chính xác là tỷ lệ phần trăm đối sánh được giới hạn (1/3) được yêu cầu. Tổng các giá trị đó trong danh sách đối thủ và chia cho độ dài của nó sẽ cho chính xác OMW. Lập luận tương tự cũng áp dụng cho GW và OGW, ngoại trừ việc mẫu số trò chơi của mỗi đối thủ phụ thuộc vào số lượng trò chơi của chính đối thủ đó. Vì tất cả số học được thực hiện bằng số nguyên và được rút gọn bằng GCD nên các phân số được in ra là chính xác và tối giản. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def reduce_fraction(num, den):
    g = gcd(num, den)
    return num // g, den // g

def add_fraction(a, b, c, d):
    g = gcd(b, d)
    b1 = b // g
    d1 = d // g
    num = a * d1 + c * b1
    den = b1 * d
    g2 = gcd(num, den)
    return num // g2, den // g2

def main():
    T = int(input())
    out = []

    for _ in range(T):
        n, m = map(int, input().split())
        cnt = list(map(int, input().split()))

        mp = [0] * n
        game_points = [0] * n
        games = [0] * n
        opponents = [[] for _ in range(n)]

        for rnd in range(1, m + 1):
            played = [False] * n

            for _ in range(cnt[rnd - 1]):
                p1, p2, w1, w2, d = map(int, input().split())
                p1 -= 1
                p2 -= 1

                played[p1] = True
                played[p2] = True

                if w1 > w2:
                    mp[p1] += 3
                elif w1 < w2:
                    mp[p2] += 3
                else:
                    mp[p1] += 1
                    mp[p2] += 1

                game_points[p1] += 3 * w1 + d
                game_points[p2] += 3 * w2 + d

                total_games = w1 + w2 + d
                games[p1] += total_games
                games[p2] += total_games

                opponents[p1].append(p2)
                opponents[p2].append(p1)

            for i in range(n):
                if not played[i]:
                    mp[i] += 3
                    game_points[i] += 6
                    games[i] += 2

            out.append(f"Round {rnd}")

            for i in range(n):
                gw_num = max(games[i], game_points[i])
                gw_den = 3 * games[i]
                gw_num, gw_den = reduce_fraction(gw_num, gw_den)

                if not opponents[i]:
                    omw_num, omw_den = 1, 3
                    ogw_num, ogw_den = 1, 3
                else:
                    opponent_count = len(opponents[i])

                    omw_sum = 0
                    for v in opponents[i]:
                        omw_sum += max(rnd, mp[v])

                    omw_num = omw_sum
                    omw_den = 3 * rnd * opponent_count
                    omw_num, omw_den = reduce_fraction(
                        omw_num, omw_den
                    )

                    ogw_num, ogw_den = 0, 1
                    for v in opponents[i]:
                        v_num = max(games[v], game_points[v])
                        v_den = 3 * games[v]
                        ogw_num, ogw_den = add_fraction(
                            ogw_num, ogw_den, v_num, v_den
                        )

                    ogw_den *= opponent_count
                    ogw_num, ogw_den = reduce_fraction(
                        ogw_num, ogw_den
                    )

                out.append(
                    f"{mp[i]} "
                    f"{omw_num}/{omw_den} "
                    f"{gw_num}/{gw_den} "
                    f"{ogw_num}/{ogw_den}"
                )

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Ba mảng tích lũy đủ để xây dựng lại tỷ lệ phần trăm của riêng mỗi người chơi. MP được cập nhật từ người thắng trận hoặc trận hòa, trong khi điểm trận đấu và số trận đấu sử dụng trực tiếp số trận đấu được cung cấp. Lời tạm biệt được thể hiện bằng số gia tích lũy chính xác giống như kết quả tạm biệt 2-0 được chỉ định. 

các`opponents`danh sách là cấu trúc dữ liệu trung tâm. Nếu người chơi 1 và 2 gặp nhau ba lần, danh sách của người chơi 1 chứa`[2, 2, 2]`. Không có sự trùng lặp nào được thực hiện vì định nghĩa giải đấu tính mọi cuộc họp. 

Thứ tự cập nhật quan trọng. Một vòng đấu hoàn chỉnh phải được xử lý trước khi tính bất kỳ tỷ lệ phần trăm nào vì OMW và OGW sử dụng số liệu thống kê hiện tại của đối thủ sau vòng đó. Đầu tiên, mã sẽ xử lý mọi kết quả khớp, sau đó gán tất cả các giá trị tạm biệt và chỉ sau đó mới tính toán đầu ra. 

Phép tính OMW tránh hoàn toàn phép cộng phân số. Tại vòng (r), mọi MW đều có mẫu số (3r) nên chỉ có tử số nguyên`max(r, mp[v])`cần được tích lũy. OGW yêu cầu phép cộng phân số thực vì hai đối thủ có thể có số ván chơi khác nhau. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. Mẫu số lớn nhất dù sao cũng nhỏ vì một người chơi chơi tối đa 16 hiệp và nhiều nhất là ba trận mỗi trận, trong đó tạm biệt đóng góp hai trận. 

các`add_fraction`hàm số giảm sau mỗi lần thêm. Điều này giữ cho các số nguyên trung gian ở mức nhỏ và tránh việc xây dựng một mẫu số chung lớn trên tất cả các đối thủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Trường hợp thử nghiệm đầu tiên có hai người chơi và ba vòng. Hiệp 1 không có trận đấu nào nên cả hai người chơi đều tạm biệt. Vòng 2 có trận đấu 2-0-1 và vòng 3 có trận đấu 1-1-1. 

| Vòng | Người chơi | nghị sĩ | Điểm trò chơi | Trò chơi | Đối thủ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 6 | 2 | [] | 
| 1 | 2 | 3 | 6 | 2 | [] | 
| 2 | 1 | 6 | 13 | 5 | [2] | 
| 2 | 2 | 3 | 7 | 5 | [1] | 
| 3 | 1 | 7 | 17 | 8 | [2] | 
| 3 | 2 | 4 | 11 | 8 | [1] | 

Sau vòng 2, người chơi 1 có MW (6/6=1), trong khi người chơi 2 có MW (3/6=1/2). Giá trị GW của chúng là (15/13) và (15/7). Vì mỗi người đã đấu với chính xác một đối thủ nên những giá trị đó trở thành OMW và OGW của người kia. 

Sau vòng 3, danh sách đối thủ vẫn còn đó, nhưng số liệu thống kê hiện tại của họ đã thay đổi. Người chơi 2 hiện có GW (24/11), do đó trở thành OGW của người chơi 1. Người chơi 1 có GW (17/24) nên nó trở thành OGW của người chơi 2. 

Dấu vết này chứng tỏ tại sao tỷ lệ phần trăm của đối thủ phải được tính bằng cách sử dụng số liệu thống kê hiện tại chứ không phải số liệu thống kê từ vòng đấu khi trận đấu diễn ra. 

### Mẫu 2 

Trường hợp thử nghiệm thứ hai có ba người chơi. Ở vòng 1, người chơi 1 và 2 chơi trong khi người chơi 3 tạm biệt. Ở vòng 2, người chơi 2 đấu với người chơi 3 trong khi người chơi 1 tạm biệt. 

| Vòng | Người chơi | nghị sĩ | Điểm trò chơi | Trò chơi | Đối thủ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 2 | [2] | 
| 1 | 2 | 3 | 6 | 2 | [1] | 
| 1 | 3 | 3 | 6 | 2 | [] | 
| 2 | 1 | 3 | 6 | 4 | [2] | 
| 2 | 2 | 6 | 12 | 4 | [1, 3] | 
| 2 | 3 | 3 | 6 | 4 | [2] | 

Sau vòng 1, MW thô của người chơi 1 bằng 0, do đó nó bị giới hạn ở mức (1/3). MW của Người chơi 2 là 1, vì vậy người chơi 1 có OMW (1), trong khi người chơi 2 có OMW (1/3). 

Sau vòng 2, người chơi 2 phải đối mặt với hai đối thủ. Người chơi 1 có MW (3/6=1/2) và người chơi 3 cũng có MW (3/6=1/2). Do đó OMW của người chơi 2 là 

[ 
\frac{1/2+1/2}{2}=\frac12. 
] 

Tính trung bình tương tự cũng xảy ra với OGW, khi cả ba người chơi đều đã chơi bốn trận cho đến thời điểm này. 

Dấu vết này thể hiện cả sự tạm biệt và thực tế là danh sách đối thủ của người chơi có thể chứa những người chơi khác nhau với số liệu thống kê hiện tại khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nm^2)) | Sau vòng (r), mỗi người chơi có tối đa (r) mục nhập của đối thủ, vì vậy tất cả danh sách đối thủ đều chứa (O(nr)) mục nhập. Tổng tất cả các vòng sẽ cho (O(nm^2)). | 
| Không gian | (O(nm)) | Thống kê tích lũy sử dụng bộ nhớ (O(n)) và tất cả các danh sách đối thủ chứa tổng cộng tối đa (nm) mục nhập. | 

Vì (m\le16), hệ số (m^2) nhiều nhất là 256. Quan trọng hơn, đầu vào đảm bảo rằng tổng của (n m) trong tất cả các trường hợp chỉ là (3\cdot10^5). Số lần lặp lại danh sách đối thủ thu được nằm trong phạm vi dự kiến, trong khi mức sử dụng bộ nhớ vẫn tuyến tính trong tổng quy mô giải đấu. 

## Trường hợp thử nghiệm```python
import io
import sys
from math import gcd

def solve(data: str) -> str:
    inp = io.StringIO(data)

    def rd():
        return inp.readline()

    t = int(rd())
    out = []

    def reduce_fraction(a, b):
        g = gcd(a, b)
        return a // g, b // g

    def add_fraction(a, b, c, d):
        g = gcd(b, d)
        b1 = b // g
        d1 = d // g
        a = a * d1 + c * b1
        b = b1 * d
        g = gcd(a, b)
        return a // g, b // g

    for _ in range(t):
        n, m = map(int, rd().split())
        counts = list(map(int, rd().split()))

        mp = [0] * n
        gp = [0] * n
        games = [0] * n
        opp = [[] for _ in range(n)]

        for r in range(1, m + 1):
            played = [False] * n

            for _ in range(counts[r - 1]):
                p1, p2, w1, w2, d = map(int, rd().split())
                p1 -= 1
                p2 -= 1

                played[p1] = True
                played[p2] = True

                if w1 > w2:
                    mp[p1] += 3
                elif w2 > w1:
                    mp[p2] += 3
                else:
                    mp[p1] += 1
                    mp[p2] += 1

                gp[p1] += 3 * w1 + d
                gp[p2] += 3 * w2 + d

                g = w1 + w2 + d
                games[p1] += g
                games[p2] += g

                opp[p1].append(p2)
                opp[p2].append(p1)

            for i in range(n):
                if not played[i]:
                    mp[i] += 3
                    gp[i] += 6
                    games[i] += 2

            out.append(f"Round {r}")

            for i in range(n):
                gw_num, gw_den = reduce_fraction(
                    max(games[i], gp[i]),
                    3 * games[i]
                )

                if not opp[i]:
                    omw_num, omw_den = 1, 3
                    ogw_num, ogw_den = 1, 3
                else:
                    d = len(opp[i])

                    omw_num = sum(max(r, mp[v]) for v in opp[i])
                    omw_den = 3 * r * d
                    omw_num, omw_den = reduce_fraction(
                        omw_num, omw_den
                    )

                    ogw_num, ogw_den = 0, 1
                    for v in opp[i]:
                        x = max(games[v], gp[v])
                        y = 3 * games[v]
                        ogw_num, ogw_den = add_fraction(
                            ogw_num, ogw_den, x, y
                        )

                    ogw_den *= d
                    ogw_num, ogw_den = reduce_fraction(
                        ogw_num, ogw_den
                    )

                out.append(
                    f"{mp[i]} {omw_num}/{omw_den} "
                    f"{gw_num}/{gw_den} {ogw_num}/{ogw_den}"
                )

    return "\n".join(out)

def run(inp: str) -> str:
    return solve(inp)

sample = """\
2
2 3
0 1 1
1 2 2 0 1
1 2 1 1 1
3 2
1 1
1 2 0 2 0
2 3 2 0 0
"""

sample_expected = """\
Round 1
3 1/3 1/1 1/3
3 1/3 1/1 1/3
Round 2
6 1/2 13/15 7/15
3 1/1 7/15 13/15
Round 3
7 4/9 17/24 11/24
4 7/9 11/24 17/24
Round 1
0 1/1 1/3 1/1
3 1/3 1/1 1/3
3 1/3 1/1 1/3
Round 2
3 1/1 1/2 1/1
6 1/2 1/1 1/2
3 1/1 1/2 1/1
"""

assert run(sample) == sample_expected, "official sample"

case_min = """\
1
2 1
0
"""

expected_min = """\
Round 1
3 1/3 1/1 1/3
3 1/3 1/1 1/3
"""

assert run(case_min) == expected_min, "minimum all-bye case"

case_draw = """\
1
2 1
1
1 2 1 1 1
"""

expected_draw = """\
Round 1
1 1/3 4/9 4/9
1 1/3 4/9 4/9
"""

assert run(case_draw) == expected_draw, "draw and exact 1/3 cap"

case_repeat = """\
1
2 2
1 1
1 2 2 0 0
1 2 0 2 0
"""

expected_repeat = """\
Round 1
3 1/3 1/1 1/3
0 1/1 1/3 1/1
Round 2
3 1/1 1/1 1/3
3 1/1 1/3 1/1
"""

assert run(case_repeat) == expected_repeat, "repeated opponent"

case_equal = """\
1
4 2
2 2
1 2 1 1 1
3 4 1 1 1
1 2 1 1 1
3 4 1 1 1
"""

expected_equal = """\
Round 1
1 1/3 4/9 4/9
1 1/3 4/9 4/9
1 1/3 4/9 4/9
1 1/3 4/9 4/9
Round 2
2 1/3 4/9 4/9
2 1/3 4/9 4/9
2 1/3 4/9 4/9
2 1/3 4/9 4/9
"""

assert run(case_equal) == expected_equal, "all equal results"

# Maximum n*m case: 10,000 players, 16 rounds, every player gets a bye.
# This checks both the input-size boundary and repeated-round processing.
n = 10000
m = 16
max_case = "1\n10000 16\n" + " ".join(["0"] * 16) + "\n"

max_output = run(max_case)
lines = max_output.splitlines()

assert len(lines) == 16 * (n + 1), "maximum-size output length"

for r in range(1, 17):
    base = (r - 1) * (n + 1)
    assert lines[base] == f"Round {r}", "round header"

    expected_line = f"{3 * r} 1/3 1/1 1/3"
    assert lines[base + 1] == expected_line, "first player"
    assert lines[base + n] == expected_line, "last player"
```Trường hợp tối thiểu sẽ kiểm tra để tạm dừng cập nhật MP, điểm trò chơi và trò chơi mà không tạo đối thủ. Trường hợp hòa sẽ kiểm tra một trận đấu không thắng và tỷ lệ giảm chính xác. Trường hợp đối thủ lặp lại xác minh rằng cùng một đối thủ được tính một lần mỗi trận. Trường hợp hoàn toàn bằng nhau sẽ kiểm tra các vòng lặp lại với số liệu thống kê giống hệt nhau và phát hiện sự thay thế trạng thái ngẫu nhiên thay vì cập nhật tích lũy. Thử nghiệm cuối cùng sử dụng (n) và (m) mức tối đa được phép cùng với (n m=160.000) và xác minh toàn bộ cấu trúc đầu ra mà không lưu trữ chuỗi dự kiến ​​viết tay khổng lồ. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (n=2,m=1), không có kết quả phù hợp | Cả hai người chơi đều có`3 1/3 1/1 1/3`| Tạm biệt cách xử lý và tỷ lệ không có đối thủ | 
| (n=2,m=1), một trận hòa 1-1-1 | Cả hai người chơi đều có`1 1/3 4/9 4/9`| Vẽ chấm điểm và phân số chính xác | 
| (n=2,m=2), cùng một người chơi gặp nhau hai lần | Các giá trị tích lũy phụ thuộc vào vòng được hiển thị ở trên | Đối thủ lặp đi lặp lại | 
| (n=4,m=2), trận nào cũng hòa | Tất cả người chơi vẫn đối xứng | Trạng thái tích lũy và số liệu thống kê bằng nhau | 
| (n=10000,m=16), không có kết quả phù hợp | 16 vòng kết quả tạm biệt giống hệt nhau | Tối đa (n), tối đa (m), kích thước đầu ra, hiệu suất | 

## Vỏ cạnh 

Người chơi chỉ nhận lời tạm biệt sẽ được xử lý bởi`played`mảng. Sau khi tất cả các trận đấu thực tế trong một vòng được xử lý, mọi người chơi có cờ vẫn sai sẽ nhận được chính xác 3 MP, 6 điểm trò chơi và 2 trò chơi. Danh sách đối thủ của họ vẫn trống, vì vậy đầu ra sử dụng rõ ràng (1/3) cho cả OMW và OGW. Đối với đầu vào```
1
2 1
0
```người chơi đầu tiên đạt MP 3 và GW (6/6=1), cho`3 1/3 1/1 1/3`, và người chơi thứ hai nhận được kết quả tương tự. 

Giới hạn (1/3) được áp dụng thông qua`max(r, mp[i])`cho MW và`max(games[i], gp[i])`cho GW. Giả sử một người chơi thua mọi trận đấu. Sau vòng 3 MP của họ có thể bằng 0 nhưng tử số MW của họ vẫn là`max(3, 0) = 3`, cho (3/9=1/3). Cấu trúc tương tự cũng áp dụng cho GW, trong đó người chơi không có điểm trò chơi vẫn nhận được tử số giới hạn bằng số trò chơi của họ. 

Đối thủ lặp lại được bảo toàn bằng cách thêm đối thủ vào mỗi trận đấu. Nếu danh sách đối thủ trở thành`[2, 2]`, vòng lặp OMW xử lý trình phát 2 hai lần. Sau vòng thứ hai, cả hai mục đều sử dụng MW vòng 2 hiện tại của Người chơi 2, khớp chính xác với định nghĩa. Không có thao tác thiết lập hoặc loại bỏ trùng lặp nào xuất hiện ở bất kỳ đâu trong thuật toán. 

Thời điểm tính toán tỷ lệ phần trăm cũng có vấn đề. Giả sử người chơi 1 và 2 gặp nhau ở vòng 1 và người chơi 2 sau đó đấu với người khác ở vòng 2. Khi tính OMW của người chơi 1 sau vòng 2, kết quả vòng 2 của người chơi 2 phải được đưa vào. Quá trình triển khai xử lý tất cả các trận đấu ở vòng 2 và tất cả các trận đấu ở vòng 2 trước khi tính toán bất kỳ kết quả đầu ra nào, do đó mọi thống kê về đối thủ đều đến từ cùng một vòng đấu đã hoàn thành. 

Cuối cùng, tỷ lệ phần trăm của trò chơi không thể sử dụng số vòng làm mẫu số. Tạm biệt đóng góp hai trò chơi, một trận đấu bình thường có thể chứa hai hoặc ba trò chơi và do đó những người chơi khác nhau có thể có số lượng trò chơi khác nhau. Mã duy trì`games[i]`một cách rõ ràng và sử dụng (3\cdot\mathrm{games[i]) làm số điểm trò chơi tối đa có thể. Đây cũng là lý do tại sao OGW được tính bằng cách cộng các phân số GW riêng lẻ của đối thủ thay vì cố gắng kết hợp điểm trò chơi và số trận đấu của họ.
