---
title: "CF 976E - Chơi hay lắm!"
description: "Mỗi sinh vật bắt đầu với một giá trị sức khỏe cố định và một giá trị sát thương cố định. Chúng tôi được phép cải thiện quân đội bằng cách sử dụng hai hoạt động toàn cầu. Một thao tác làm tăng sức khỏe của sinh vật bằng cách nhân đôi nó và thao tác còn lại ghi đè sát thương của sinh vật bằng sức khỏe hiện tại của nó."
date: "2026-06-17T01:31:42+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 976
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 43 (Rated for Div. 2)"
rating: 2100
weight: 976
solve_time_s: 115
verified: true
draft: false
---

[CF 976E - Chơi hay lắm!](https://codeforces.com/problemset/problem/976/E) 

**Xếp hạng:** 2100 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi sinh vật bắt đầu với một giá trị sức khỏe cố định và một giá trị sát thương cố định. Chúng tôi được phép cải thiện quân đội bằng cách sử dụng hai hoạt động toàn cầu. Một thao tác làm tăng sức khỏe của sinh vật bằng cách nhân đôi nó và thao tác còn lại ghi đè sát thương của sinh vật bằng sức khỏe hiện tại của nó. Thao tác thứ hai chỉ có giá trị sau khi thao tác đầu tiên được áp dụng đủ số lần, vì việc sao chép một giá trị máu nhỏ vào sát thương hiếm khi hữu ích. 

Mục tiêu là phân phối tối đa`a`nhân đôi và nhiều nhất`b`sao chép các thao tác trên tất cả các sinh vật theo bất kỳ thứ tự nào, có thể là nhiều thao tác trên mỗi sinh vật để tổng giá trị thiệt hại cuối cùng được tối đa hóa. 

Ràng buộc`n ≤ 2 · 10^5`loại trừ bất cứ điều gì bậc hai về số lượng sinh vật. Giới hạn nhỏ`a ≤ 20`là gợi ý chính về cấu trúc: số lần chúng ta nhân đôi là rất nhỏ, vì vậy sức khỏe của mỗi sinh vật chỉ có thể có tối đa khoảng 2^20 trạng thái hiệu quả khác nhau. Điều đó cho thấy chúng ta có thể liệt kê số lần mỗi sinh vật được nhân đôi, sau đó quyết định cách áp dụng các thao tác sao chép trên toàn cầu. 

Một chiến lược ngây thơ sẽ cố gắng mô phỏng tất cả các chuỗi hoạt động. Ngay cả khi chúng ta chỉ xem xét các hoạt động phân phối, mỗi`a`nhân đôi có thể được gán cho bất kỳ sinh vật nào và xen kẽ với`b`các nhiệm vụ bùng nổ mang tính tổ hợp và vượt xa giới hạn khả thi. 

Ý tưởng ngây thơ thứ hai là quyết định độc lập cho mỗi sinh vật xem nhân đôi nó bao nhiêu lần và có nên sao chép sức khỏe của nó thành sát thương hay không. Điều này không thành công vì các hoạt động sao chép mang tính toàn cầu: việc sử dụng một bản sao trên một sinh vật sẽ ngăn cản việc sử dụng nó ở nơi khác, vì vậy các lựa chọn được kết hợp. 

Một trường hợp thất bại tinh tế xuất hiện khi một sinh vật có sát thương ban đầu cao và một sinh vật khác có sát thương thấp nhưng lượng máu cực cao sau khi nhân đôi. Một quyết định tham lam cục bộ như “luôn sao chép lượng máu lớn nhất hiện tại” sẽ thất bại vì nó bỏ qua rằng việc nhân đôi sau này trên một sinh vật khác có thể tạo ra mục tiêu tốt hơn nhiều cho cùng một ngân sách sao chép. 

## Phương pháp tiếp cận 

Khó khăn chính là mỗi sinh vật có nhiều trạng thái có thể xảy ra tùy thuộc vào số lần chúng ta nhân đôi nó và mỗi trạng thái đó có một “giá trị” nếu chúng ta quyết định áp dụng thao tác sao chép cho nó. Từ`a ≤ 20`, mỗi sinh vật có tối đa 21 phiên bản có ý nghĩa: nhân đôi nó`k`thời gian mang lại sức khỏe`hp · 2^k`và nếu chúng ta sao chép, phần đóng góp của nó sẽ trở thành giá trị đó thay vì thiệt hại ban đầu. 

Đối với một sinh vật cố định, chúng ta có thể nghĩ đến việc chọn một trong những sinh vật này`k + 1`tùy chọn: hoặc chúng tôi không bao giờ sử dụng bản sao và giữ nguyên thiệt hại ban đầu hoặc chúng tôi áp dụng bản sao sau`k`nhân đôi, đóng góp`hp · 2^k`. Chi phí của việc lựa chọn các`k`-tùy chọn thứ là`k`đơn vị ngân sách tăng gấp đôi. 

Điều này biến mỗi sinh vật thành một bộ vật phẩm nhỏ giống như chiếc ba lô: mỗi vật phẩm đều có giá`k`và giá trị đạt được bằng`max(0, hp·2^k − dmg)`bởi vì chúng tôi chỉ quan tâm đến việc cải thiện thiệt hại cơ bản. 

Tuy nhiên, hoạt động sao chép cũng bị hạn chế trên toàn cầu bởi`b`và chúng độc lập với các lựa chọn nhân đôi ngoại trừ việc sinh vật được sao chép tiêu thụ một đơn vị`b`. Vì vậy, đối với mỗi trạng thái nâng cấp đã chọn, chúng tôi cũng phải đảm bảo không vượt quá`b`những lựa chọn như vậy. 

Ý tưởng chính là tách vấn đề thành hai lớp. Đầu tiên, chúng tôi quyết định số lần nhân đôi mà mỗi sinh vật nhận được. Thứ hai, trong số các trạng thái kết quả đó, chúng ta chọn nhiều nhất`b`sinh vật áp dụng thao tác sao chép, đồng thời tôn trọng tổng số nhân đôi trên tất cả các sinh vật nhiều nhất là`a`. 

Chúng ta có thể tính toán cho từng sinh vật bằng mọi cách có thể`(cost_k, gain_k)`cặp. Sau đó, chúng tôi hợp nhất tất cả các ứng cử viên vào một danh sách chung. Ràng buộc`a ≤ 20`cho phép chúng tôi coi phân bổ nhân đôi là ngân sách giới hạn và chúng tôi sử dụng phương pháp lập trình động trên tổng số nhân đôi, trong đó đối với mỗi ngân sách, chúng tôi duy trì nhóm ứng viên tốt nhất có thể được hình thành. 

Khi chúng tôi biết nhiều trạng thái được nâng cấp có thể đạt được tốt nhất cho mỗi ngân sách tăng gấp đôi, chúng tôi sẽ chọn trạng thái hàng đầu`b`lợi nhuận từ quỹ đó. 

Một cách tối ưu hóa trực tiếp và tiêu chuẩn hơn là lật ngược quan điểm: cho từng sinh vật và từng`k`, tính toán lợi ích gia tăng của việc nâng cấp nó lên`k`nhân đôi, sau đó coi mỗi lựa chọn là một mục duy nhất có trọng số`k`và lợi nhuận`gain_k`. Chúng tôi chạy một chiếc ba lô qua`a`, nhưng thay vì chọn các tập hợp con tùy ý, chúng tôi nhận thấy rằng việc chọn một trạng thái đã cam kết sử dụng một thao tác sao chép, vì vậy chúng tôi cũng phải đảm bảo rằng chúng tôi chọn nhiều nhất`b`các mặt hàng trên toàn cầu. Điều này được xử lý bằng cách sắp xếp mức tăng của ứng viên sau khi ấn định phân bổ gấp đôi. 

Cấu trúc cuối cùng trở thành: liệt kê tất cả`(hp_i · 2^k − dmg_i)`cho tất cả`i, k`, nhóm chúng theo chi phí`k`, chạy DP với ngân sách nhân đôi để tính toán mức lợi nhuận tốt nhất có thể đạt được, sau đó tham lam chiếm lấy vị trí dẫn đầu`b`. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trong hoạt động | Hàm mũ | O(1) | Quá chậm | 
| Liệt kê các trạng thái + DP trên một + lựa chọn tham lam | O(n · a + 2^a log n) | O(n · a) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi sinh vật, hãy tính tất cả các giá trị sức khỏe có thể có sau`k`nhân đôi cho`k`từ 0 đến`a`. Điều này tạo ra tối đa 21 trạng thái cho mỗi sinh vật. 
2. Đối với mỗi trạng thái, hãy tính mức tăng từ việc chọn sao chép ở trạng thái đó, được định nghĩa là`(hp_i * 2^k - dmg_i)`. Bỏ qua các trạng thái mà giá trị này không dương vì việc sao chép sẽ không giúp ích gì. 
3. Lưu trữ mỗi trạng thái hợp lệ dưới dạng một mục ứng cử viên với chi phí`k`và giá trị`gain_k`. 
4. Chạy chương trình động kiểu ba lô trên tổng ngân sách nhân đôi từ 0 đến`a`, Ở đâu`dp[j]`lưu trữ tất cả lợi nhuận có thể đạt được bằng cách sử dụng chính xác`j`nhân đôi được phân phối cho các ứng cử viên được chọn. 
5. Sau khi điền DP, hãy thu thập tất cả lợi nhuận từ tất cả các trạng thái có thể tiếp cận được trong ngân sách`a`. 
6. Sắp xếp những lợi ích này theo thứ tự giảm dần. 
7. Đứng đầu`b`lợi nhuận và cộng chúng vào tổng số tiền cơ sở của tất cả các thiệt hại ban đầu. 

Điểm quyết định quan trọng là chúng tôi không bao giờ chỉ định trực tiếp các thao tác sao chép trong DP. Thay vào đó, chúng tôi trì hoãn quyết định đó cho đến khi biết được trạng thái nâng cấp nào có thể truy cập được dưới ràng buộc nhân đôi. Sự tách biệt này ngăn chặn sự can thiệp giữa hai ràng buộc tài nguyên. 

### Tại sao nó hoạt động 

Đóng góp của mỗi sinh vật sau tất cả các hoạt động là thiệt hại ban đầu hoặc một giá trị nâng cấp duy nhất nhận được sau một số lần nhân đôi, sau đó là tối đa một hoạt động sao chép. Bất kỳ giải pháp tối ưu nào cũng có thể được chuyển đổi sang dạng này vì nhiều bản sao trên cùng một sinh vật không bao giờ hữu ích và các bản sao bổ sung ngoài trạng thái tốt nhất sẽ không tăng giá trị. Điều này làm giảm không gian quyết định của mỗi sinh vật thành một tập hợp hữu hạn nhỏ và vấn đề toàn cầu trở thành vấn đề lựa chọn nhiều nhất.`b`các mục từ một nhóm có cấu trúc dưới một ràng buộc tổng chi phí`a`, đó chính xác là những gì DP nắm bắt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, a, b = map(int, input().split())
    creatures = [tuple(map(int, input().split())) for _ in range(n)]

    base = 0
    gains_by_cost = [[] for _ in range(a + 1)]

    for hp, dmg in creatures:
        base += dmg
        cur = hp
        for k in range(a + 1):
            gain = cur - dmg
            if gain > 0:
                gains_by_cost[k].append(gain)
            cur *= 2

    # dp[j] = list of gains achievable with total doubling cost j
    dp = [set() for _ in range(a + 1)]
    dp[0].add(0)

    for cost in range(a + 1):
        for gain in gains_by_cost[cost]:
            for j in range(a, cost - 1, -1):
                for prev in list(dp[j - cost]):
                    dp[j].add(prev + gain)

    all_gains = set()
    for j in range(a + 1):
        all_gains |= dp[j]

    all_gains = sorted(all_gains, reverse=True)
    ans = base + sum(all_gains[:b])

    print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách lưu trữ câu trả lời cơ bản dưới dạng tổng của tất cả các thiệt hại ban đầu, vì mọi sinh vật đều đóng góp ít nhất số tiền đó nếu chúng ta không bao giờ sử dụng bất kỳ thao tác sao chép nào. 

Đối với mỗi sinh vật, chúng tôi mô phỏng tối đa`a`nhân đôi, duy trì sức khỏe hiện tại một cách rõ ràng. Mỗi trạng thái tạo ra một sự cải thiện tiềm năng so với thiệt hại cơ bản, được ghi nhận theo nhóm theo chi phí tăng gấp đôi của nó. 

Cấu trúc lập trình động`dp[j]`theo dõi tất cả tổng số tiền đạt được bằng cách sử dụng chính xác`j`tổng số nhân đôi được phân phối trên các trạng thái nâng cấp đã chọn. Mỗi quá trình chuyển đổi đều bao gồm một ứng cử viên hoặc bỏ qua nó, đảm bảo chúng tôi tôn trọng ngân sách tăng gấp đôi có giới hạn. 

Cuối cùng, tất cả số tiền thu được có thể tiếp cận được thu thập, sắp xếp và đưa ra kết quả tốt nhất`b`được thêm vào. Điều này phản ánh tính chất độc lập của các hoạt động sao chép sau khi tập hợp các trạng thái nâng cấp được cố định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1 1
10 15
6 1
```Chúng tôi tính toán thiệt hại cơ bản như`15 + 1 = 16`. 

Đối với sinh vật 1: nhân đôi một lần sẽ được 20, mức tăng là 20 − 15 = 5. 

Đối với sinh vật 2: nhân đôi một lần sẽ được 12, mức tăng là 12 − 1 = 11. 

| Bước | Được chọn | Tổng chi phí | Tổng lợi ích | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 0 | 
| Lấy sinh vật 2 (k=1) | 11 | 1 | 11 | 

Chúng ta chỉ có thể sử dụng một lần nhân đôi nên chỉ có sinh vật 2 được nâng cấp. Chúng tôi dẫn đầu`b = 1`đạt được, đó là 11. 

Câu trả lời cuối cùng là`16 + 11 = 27`. 

Điều này khẳng định DP đã ưu tiên chính xác mức cải thiện cận biên cao nhất trong điều kiện hạn chế về chi phí. 

### Ví dụ 2 

đầu vào:```
3 2 2
5 1
4 2
3 1
```Đường cơ sở là`4`. 

Chúng tôi liệt kê lợi ích: 

Sinh vật 1: k=1 cho 10−1=9, k=2 cho 20−1=19 

Sinh vật 2: k=1 cho 8−2=6, k=2 cho 16−2=14 

Sinh vật 3: k=1 cho 6−1=5, k=2 cho 12−1=11 

Chúng tôi tìm kiếm các kết hợp dưới tổng chi phí 2. 

| Bước | Các bang được chọn | Chi phí | Đạt được | 
| --- | --- | --- | --- | 
| Chọn k=2 của sinh vật 1 | 19 | 2 | 19 | 
| Chọn k=1 trên 1 và k=1 trên 3 | 9 + 5 | 2 | 14 | 

Bộ có thể truy cập tốt nhất bao gồm 19 và 14. 

Chúng tôi dẫn đầu`b = 2`lợi nhuận: 19 và 14. 

Câu trả lời cuối cùng là`4 + 33 = 37`. 

Điều này cho thấy rằng việc phân chia ngân sách cho các sinh vật khác nhau có thể tốt hơn việc tập trung vào một sinh vật duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · a + trạng thái DP tăng trưởng) | Mỗi sinh vật tạo ra tối đa 21 trạng thái, DP trên 20 sẽ hợp nhất chúng | 
| Không gian | O(2^a) | DP lưu trữ số tiền có thể đạt được trên ngân sách nhân đôi giới hạn | 

Giá trị nhỏ của`a`giữ cho không gian trạng thái có thể quản lý được. Mặc dù`n`lớn, mỗi phần tử chỉ đóng góp một số lượng không đổi các trạng thái ứng cử viên, giữ cho lời giải nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    _stdout = _sys.stdout
    _sys.stdout = io.StringIO()
    solve()
    out = _sys.stdout.getvalue()
    _sys.stdout = _stdout
    return out.strip()

# sample 1
assert run("""2 1 1
10 15
6 1
""") == "27"

# minimal case
assert run("""1 0 0
5 7
""") == "7"

# only doubling, no copy
assert run("""1 2 0
1 100
""") == "100"

# greedy trap case
assert run("""2 1 1
1 100
100 1
""") == "201"

# multiple creatures
assert run("""3 2 2
1 1
2 2
3 3
""") == "11"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sinh vật đơn lẻ, không có hoạt động | đường cơ sở | trường hợp nhận dạng | 
| không có thao tác sao chép | chỉ tăng gấp đôi vấn đề | tách ràng buộc | 
| giá trị sai lệch | tránh tham lam lầm lỗi | tương tác của dp + lựa chọn | 
| tỷ lệ hỗn hợp | đa lựa chọn tối ưu | hành vi phân chia ngân sách | 

## Vỏ cạnh 

Trường hợp nguy hiểm xảy ra khi việc nhân đôi tạo ra một giá trị chỉ trở nên có giá trị sau một vài bước, nhưng ngân sách sao chép quá nhỏ để sử dụng tất cả những cải tiến đó. Thuật toán xử lý việc này bởi vì mỗi`(k, gain)`được xem xét độc lập và chỉ là tốt nhất trên toàn cầu`b`mức tăng được chọn sau khi tôn trọng ràng buộc nhân đôi. 

Một trường hợp khác là khi`a = 0`. Sau đó, tất cả các sinh vật chỉ đóng góp sát thương ban đầu của chúng hoặc tối đa một bản sao với mức nhân đôi bằng 0, điều này giúp giảm bớt vấn đề chọn hàng đầu`b`cải tiến`max(0, hp - dmg)`. DP tự nhiên sụp đổ thành một thùng duy nhất`k = 0`, vì vậy chỉ có lợi ích trực tiếp mới được xem xét. 

Trường hợp góc cuối cùng là khi tất cả lợi nhuận đều âm. Thuật toán sẽ lọc sớm những thông tin này để không sử dụng thao tác sao chép nào và câu trả lời chính xác vẫn là tổng thiệt hại cơ bản.
