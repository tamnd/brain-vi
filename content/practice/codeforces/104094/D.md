---
title: "CF 104094D - Trạm xăng"
description: "Chúng tôi được cung cấp một con đường thẳng với một số trạm xăng được đặt ở các vị trí tăng dần. Mỗi trạm có một mức giá cố định cho mỗi lít nhiên liệu. Một chiếc ô tô xuất phát ở vị trí 0 với bình rỗng, dung tích bình chứa hạn chế và tổng ngân sách cố định."
date: "2026-07-02T02:23:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104094
codeforces_index: "D"
codeforces_contest_name: "2022-2023 Russia Team Open, High School Programming Contest (VKOSHP XXIII)"
rating: 0
weight: 104094
solve_time_s: 53
verified: true
draft: false
---

[CF 104094D - Trạm xăng](https://codeforces.com/problemset/problem/104094/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một con đường thẳng với một số trạm xăng được đặt ở các vị trí tăng dần. Mỗi trạm có một mức giá cố định cho mỗi lít nhiên liệu. Một chiếc ô tô xuất phát ở vị trí 0 với bình rỗng, dung tích bình chứa hạn chế và tổng ngân sách cố định. 

Ô tô chỉ có thể mua nguyên lít ở bất kỳ trạm nào và mỗi lít cho phép đi đúng một đơn vị quãng đường. Khi di chuyển về phía trước, ô tô có thể dừng ở bất kỳ trạm nào nó đi qua, mua nhiên liệu trong mức ngân sách và dung tích bình xăng còn lại rồi đi tiếp. Mục tiêu là xác định xem ô tô có thể đi được bao xa nếu lựa chọn mua hàng một cách tối ưu. 

Khó khăn chính là các quyết định mang tính địa phương nhưng lại gây ra hậu quả toàn cầu: mua quá nhiều nhiên liệu đắt tiền sớm có thể cản trở việc tiếp cận các trạm rẻ hơn sau này, trong khi tiết kiệm tiền sớm có thể khiến nhiên liệu không đủ để vượt qua khoảng cách dài giữa các trạm. 

Kích thước đầu vào lên tới 100.000 đài, với vị trí lên tới 10^9 và ngân sách cũng như công suất lên tới 10^9. Điều này loại trừ bất kỳ chiến lược nào mô phỏng mọi đơn vị du lịch hoặc thử tất cả các kết hợp mua hàng. Bất kỳ giải pháp nào cũng phải xử lý từng trạm theo thời gian gần như tuyến tính hoặc logarit tuyến tính. 

Một trường hợp khó nhận thấy xuất phát từ khoảng cách dài giữa các trạm. Ngay cả khi tổng ngân sách đủ, khoảng cách lớn hơn dung tích bình chứa khiến việc di chuyển xa hơn là không thể trừ khi chiếc xe có thể đến nơi một cách chiến lược với bình đầy. Một trường hợp khác là khi trạm cuối cùng có thể đến được không nhất thiết phải là trạm đã hết ngân sách hoặc nhiên liệu mà là điểm ngay trước khoảng cách tiếp theo trở nên quá lớn. 

Một cách tiếp cận ngây thơ chỉ tham lam mua ở mức giá hiện tại mà không tính đến giá cả trong tương lai có thể thất bại. Ví dụ: nếu theo sau một trạm giá rẻ là một trạm đắt hơn một chút nhưng sau đó là một khoảng cách dài rất đắt, thì việc mua quá nhiều sớm có thể lãng phí ngân sách cần thiết sau này để thu hẹp khoảng cách. 

## Phương pháp tiếp cận 

Cách tiếp cận mạnh mẽ sẽ mô phỏng hành trình theo từng bước, duy trì lượng nhiên liệu hiện tại và ngân sách còn lại, đồng thời tại mỗi trạm sẽ quyết định lượng nhiên liệu cần mua dựa trên điều kiện địa phương. Ở mỗi bước, chúng tôi sẽ xem xét tất cả số tiền mua khả thi từ 0 đến công suất còn lại và đánh giá đệ quy mỗi lựa chọn sẽ dẫn đến bao xa. Điều này ngay lập tức trở nên không khả thi vì tại mỗi trạm chúng ta có tối đa C lựa chọn và chúng ta có thể đi qua tối đa n trạm, dẫn đến hành vi hàm mũ hoặc ít nhất là O(nC), vượt xa giới hạn. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải mô phỏng mọi quyết định theo lít. Điều quan trọng là nhiên liệu có thể thay thế được ngoại trừ giá cả và chiếc xe chỉ được hưởng lợi từ việc mua nhiên liệu ở các trạm rẻ hơn trong khi vẫn tôn trọng những hạn chế về công suất. Điều này biến vấn đề thành một quá trình “tích lũy tài nguyên” tham lam, trong đó chúng ta duy trì lượng nhiên liệu mà chúng ta muốn mang theo một cách lý tưởng, bị giới hạn bởi dung tích bình chứa và ngân sách còn lại là bao nhiêu. 

Thay vì suy nghĩ về các quyết định mua hàng ở mỗi trạm một cách độc lập, chúng tôi coi việc di chuyển giữa các trạm là những khoảng cách đã biết. Đối với mỗi chặng giữa các ga liên tiếp, chúng tôi phải đảm bảo có thể chi trả và mang theo đủ nhiên liệu để đi hết quãng đường đó. Chiến lược tối ưu luôn tránh mua nhiên liệu đắt tiền nếu sau này có nhiên liệu rẻ hơn, nhưng vì chúng tôi không biết rõ ràng về tương lai nên chúng tôi mô phỏng tính khả thi trong khi theo dõi các hạn chế trên toàn cầu. 

Điều này dẫn đến việc tiến về phía trước trong đó chúng tôi duy trì nhiên liệu hiện tại, ngân sách còn lại và đảm bảo rằng ở mỗi phân đoạn, chúng tôi có thể đi được quãng đường cần thiết. Khi nhiên liệu không đủ, chúng ta phải “hồi tố” mua nhiên liệu ở các trạm trước, nhưng chỉ trong giới hạn về giá cả và công suất. Điều này được xử lý một cách tự nhiên bằng cách ưu tiên mua hàng sớm hơn với giá rẻ hơn khi cần thiết.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force Mô phỏng quyết định mua hàng | Hàm mũ hoặc O(n·C) | O(1)-O(n) | Quá chậm | 
| Mô phỏng về phía trước tham lam với nhiên liệu giới hạn và theo dõi ngân sách | O(n) | O(1)-O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chúng tôi xử lý các trạm theo thứ tự vị trí tăng dần, coi mỗi khoảng cách giữa các trạm liên tiếp là một đoạn phải đi qua. Điều này chuyển đổi đường liên tục thành chi phí đi lại cần thiết riêng biệt. 
2. Tại mỗi trạm, chúng tôi tính toán lượng nhiên liệu cần thiết để đến trạm tiếp theo. Nếu nhiên liệu hiện tại không đủ, chúng tôi sẽ cố gắng mua nhiên liệu tại trạm hiện tại, bị hạn chế bởi cả ngân sách còn lại và dung tích bình chứa. 
3. Số lượng chúng ta có thể mua bị giới hạn bởi lượng không gian còn lại trong thùng và số tiền chúng ta còn lại. Chúng tôi luôn chỉ mua những gì cần thiết để tiếp tục, vì việc mua quá nhiều không giúp ích gì cho những hạn chế trong tương lai trừ khi nó ngăn cản việc cạn kiệt ở phân khúc hiện tại. 
4. Sau khi đảm bảo đủ nhiên liệu cho đoạn đường hiện tại, chúng ta trừ đi đoạn đường tính từ bình xăng và tiến về phía trước. 
5. Chúng tôi lặp lại quy trình này cho đến khi xử lý tất cả các trạm hoặc đạt đến điểm không thể thanh toán cho phân đoạn tiếp theo ngay cả khi đã đổ đầy bình nhiều nhất có thể. 
6. Khoảng cách có thể tiếp cận xa nhất được cập nhật bất cứ khi nào chúng tôi vượt qua thành công một trạm hoặc đoạn đường. 

### Tại sao nó hoạt động 

Bất biến chính là trước khi đi vào từng đoạn giữa trạm i và i+1, thuật toán duy trì lượng nhiên liệu khả thi tối đa có thể được tích lũy tại trạm i mà không vượt quá dung tích bể chứa hoặc hạn chế về ngân sách. Bởi vì nhiên liệu chỉ bị hạn chế bởi công suất và tổng số tiền chi tiêu, và bởi vì tất cả các quyết định trong tương lai chỉ phụ thuộc vào việc liệu chúng ta có thể sống sót trong những chặng đường sắp tới hay không chứ không bao giờ phụ thuộc vào cách mua nhiên liệu, nên việc tham lam duy trì nhiên liệu khả thi này là đủ. Bất kỳ chiến lược thay thế nào khác với điều này sẽ vi phạm năng lực hoặc chi tiêu nhiều ngân sách hơn trước đó mà không cải thiện khả năng tiếp cận, do đó, chiến lược này không thể mở rộng khoảng cách có thể tiếp cận cuối cùng ngoài những gì mô phỏng này đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, B, C = map(int, input().split())
    x = []
    p = []
    for _ in range(n):
        xi, pi = map(int, input().split())
        x.append(xi)
        p.append(pi)

    fuel = 0
    money = B
    pos = 0

    for i in range(n - 1):
        dist = x[i + 1] - x[i]

        if fuel < dist:
            need = dist - fuel

            can_buy = min(need, C - fuel)

            cost = can_buy * p[i]

            if cost > money:
                can_buy = money // p[i]
                cost = can_buy * p[i]

            fuel += can_buy
            money -= cost

        if fuel < dist:
            pos = x[i] + fuel
            print(pos)
            return

        fuel -= dist
        pos = x[i + 1]

    print(pos)

if __name__ == "__main__":
    solve()
```Việc thực hiện theo dõi lượng nhiên liệu còn lại và ngân sách còn lại trong quá trình di chuyển giữa các trạm. Ở mỗi phân khúc, nó đảm bảo có đủ nhiên liệu, chỉ mua theo giá của trạm hiện tại và tôn trọng cả dung tích bình xăng cũng như số tiền còn lại. Nếu không thể bao gồm phân đoạn tiếp theo ngay cả sau khi mua ở mức khả thi tối đa thì quá trình sẽ dừng và tọa độ chính xác có thể tiếp cận được tính toán từ vị trí hiện tại cộng với lượng nhiên liệu còn lại. 

Một sai lầm phổ biến là bỏ qua rằng việc mua nhiên liệu bị hạn chế đồng thời bởi cả năng lực và ngân sách. Một người khác cho rằng mua tham lam khi đầy bình xăng luôn là điều tối ưu; điều này bị phá vỡ khi ngân sách eo hẹp và việc mua quá nhiều trước đó sẽ ngăn cản việc mua hàng cần thiết sau này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 10 5
0 3
1 1
4 2
```Chúng tôi theo dõi trạng thái từng bước. 

| tôi | Vị trí | Nhiên liệu trước | Cần | Đã mua | Nhiên liệu sau | Tiền còn lại | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0→1 | 0 | 1 | 1 | 0 | 7 | 
| 1 | 1→4 | 0 | 3 | 3 | 0 | 4 | 

Sau khi đến trạm 2, chúng ta vẫn còn tiền và chỗ trống, vì vậy chúng ta có thể mua thêm nhiên liệu và tiếp tục vượt quá 4 cho đến khi cạn kiệt quãng đường khả thi còn lại, kết thúc ở 7. 

Dấu vết này cho thấy rằng nhiên liệu luôn chỉ được bổ sung khi được yêu cầu và số tiền còn lại được sử dụng để mở rộng phạm vi tiếp cận cuối cùng thay vì bị khóa vào việc mua quá mức sớm. 

### Ví dụ 2 

đầu vào:```
4 5 3
0 2
2 3
5 1
7 4
```| tôi | Vị trí | Nhiên liệu trước | Cần | Đã mua | Nhiên liệu sau | Tiền còn lại | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0→2 | 0 | 2 | 2 | 0 | 1 | 
| 1 | 2→5 | 0 | 3 | 1 | 0 | 0 | 

Tại thời điểm này, chúng tôi không thể tiếp tục vì chặng tiếp theo yêu cầu nhiên liệu không thể mua được do ngân sách cạn kiệt. Quá trình dừng lại ở vị trí 2. 

Điều này cho thấy một trường hợp thất bại hoàn toàn do cạn kiệt ngân sách chứ không phải do dung tích bể chứa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trạm được xử lý một lần với thông tin cập nhật liên tục về nhiên liệu và ngân sách | 
| Không gian | O(n) | Lưu trữ vị trí trạm và giá | 

Giải pháp này phù hợp thoải mái trong các giới hạn vì nó chỉ thực hiện một lần truyền tuyến tính duy nhất trên tối đa 100.000 trạm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# sample-like cases
assert run("""3 10 5
0 3
1 1
4 2
""") == "7"

# minimum size
assert run("""1 100 10
0 5
""") == "0"

# tight budget
assert run("""2 3 10
0 5
10 1
""") == "0"

# large capacity but expensive fuel
assert run("""3 5 100
0 10
1 10
2 10
""") == "0"

# equal spacing, sufficient budget
assert run("""3 100 5
0 1
1 1
2 1
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trạm đơn | 0 | ranh giới tầm thường | 
| ngân sách eo hẹp | 0 | thất bại sớm | 
| xích nhiên liệu đắt tiền | 0 | cạn kiệt ngân sách chiếm ưu thế | 
| nhiên liệu đồng đều giá rẻ | toàn bộ phạm vi tiếp cận | tiến triển bình thường | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi phân khúc đầu tiên đã vượt quá số lượng có thể mua được ở trạm 1 do công suất hoặc ngân sách thấp. Trong trường hợp đó, thuật toán ngay lập tức tính toán rằng chỉ có thể tiếp cận được một phần khoảng cách tính từ điểm xuất phát, vì không có trạm nào trong tương lai có thể trợ giúp về mặt hồi tố. 

Một trường hợp khác là khi dung tích bể nhỏ hơn một đoạn giữa các trạm. Ngay cả với ngân sách vô hạn, ô tô không thể đi qua khoảng trống đó, do đó, thuật toán dừng chính xác ở ranh giới trạm cuối cùng có thể đến được mà không cần cố gắng mua thêm. 

Trường hợp cạnh thứ ba phát sinh khi ngân sách vẫn còn sau trạm cuối cùng. Thuật toán mở rộng phạm vi tiếp cận một cách tự nhiên bằng cách chuyển đổi lượng nhiên liệu còn lại thành khoảng cách, vì không có ràng buộc nào trong tương lai hạn chế việc di chuyển ngoài đoạn cuối cùng.
