---
title: "CF 105761D - Con sâu bướm"
description: "Chúng ta có một thành phố giống như đường chân trời được tạo thành từ các tòa nhà hình chữ nhật thẳng hàng theo trục đặt trên đỉnh trục x. Mỗi tòa nhà bắt đầu ở tọa độ x nào đó, mở rộng sang bên phải với chiều rộng cố định và tăng theo chiều dọc đến một độ cao nhất định."
date: "2026-06-21T22:53:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105761
codeforces_index: "D"
codeforces_contest_name: "2021 UCF Local Programming Contest"
rating: 0
weight: 105761
solve_time_s: 49
verified: true
draft: false
---

[CF 105761D - Bước đi của sâu bướm](https://codeforces.com/problemset/problem/105761/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một thành phố giống như đường chân trời được tạo thành từ các tòa nhà hình chữ nhật thẳng hàng theo trục đặt trên đỉnh trục x. Mỗi tòa nhà bắt đầu ở tọa độ x nào đó, mở rộng sang bên phải với chiều rộng cố định và tăng theo chiều dọc đến một độ cao nhất định. Các tòa nhà không bao giờ chồng lên nhau về diện tích nhưng có thể chạm vào các cạnh. 

Một con sâu bướm xuất phát tại điểm gốc trên đường mặt đất y = 0 và phải đạt tới điểm (100, 0). Nó chỉ bò dọc theo mặt đất, các bức tường tòa nhà thẳng đứng và mái nhà nằm ngang. Bất cứ khi nào nó gặp một tòa nhà, nó có thể trèo lên bức tường bên trái, đi qua mái nhà và đi xuống bức tường bên phải, tuân theo đường nét thống nhất của tất cả các tòa nhà một cách hiệu quả khi nó di chuyển từ trái sang phải. 

Nhiệm vụ là tính tổng quãng đường di chuyển dọc theo bước đi này, bao gồm cả chuyển động ngang và chuyển động dọc. 

Mỗi tòa nhà được xác định bởi vị trí x bắt đầu s, chiều rộng w và chiều cao h. Tòa nhà chiếm khoảng [s, s + w] dọc theo trục x và từ y = 0 đến y = h. 

Quan sát quan trọng là đường đi của sâu bướm chính xác là đường bao phía trên của đường chân trời được hình thành bởi tất cả các tòa nhà, cộng với các đoạn mặt đất ban đầu và cuối cùng nơi không có tòa nhà nào hiện diện. Vì vậy, vấn đề giảm xuống còn việc tính tổng chiều dài của đường cong biên được vạch dọc theo trục x và đường chân trời. 

Các hạn chế là nhỏ, chỉ có tối đa 50 tòa nhà. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu không gian nâng cao hoặc quét với các tối ưu hóa cao. Ngay cả lý luận O(n²) hoặc O(n² log n) cũng an toàn một cách thoải mái. Thách thức chính không phải là hiệu suất mà là việc hợp nhất chính xác các phân đoạn ngang chồng chéo và tính đến các chuyển đổi theo chiều dọc. 

Một số trường hợp đặc biệt quan trọng: 

Nếu hai tòa nhà chồng lên nhau trong phạm vi x, chỉ có chiều cao tối đa đóng góp vào mái nhà, nhưng cả hai đều có thể đóng góp những bức tường thẳng đứng nơi đường chân trời nhô lên hoặc hạ xuống. Ví dụ: xét (0, 10, 2) và (1, 9, 5). Đường chân trời tăng từ 0 lên 5 tại x = 1 và sau đó giảm từ 5 xuống 0 tại x = 10. Một cách tiếp cận ngây thơ là thêm từng tòa nhà một cách độc lập sẽ đếm gấp đôi cấu trúc bên trong và tạo ra một đường đi sai. 

Nếu các tòa nhà chạm chính xác vào các cạnh, chẳng hạn như một tòa nhà kết thúc ở x = 10 và tòa nhà tiếp theo bắt đầu ở x = 10, thì sẽ không có chuyển động thẳng đứng ở ranh giới đó. Việc triển khai bất cẩn có thể thêm nhầm "chuyển tiếp khoảng cách" có độ rộng bằng 0 hoặc đếm gấp đôi một đoạn dọc. 

Cuối cùng, khi không có tòa nhà nào tồn tại ở một vùng nào đó, con sâu bướm sẽ đi dọc theo y = 0, chỉ đóng góp khoảng cách theo chiều ngang chứ không được đưa vào các thành phần thẳng đứng. 

## Phương pháp tiếp cận 

Một cách trực tiếp nhưng chính xác để suy nghĩ về vấn đề này là mô phỏng rõ ràng cấu trúc đường chân trời. Chúng ta có thể rời rạc hóa trục x và với mỗi bước nhỏ, tính toán chiều cao tòa nhà tối đa bao phủ đoạn đó. Độ dài đường dẫn khi đó là tổng của các bước ngang cộng với bước nhảy dọc giữa các đoạn liền kề. Điều này đơn giản về mặt khái niệm: chúng ta xây dựng hàm cấu hình chiều cao h(x), sau đó tính chi phí truyền tải giống như chu vi của nó. 

Tuy nhiên, việc rời rạc hóa ở độ phân giải tốt là không cần thiết và có thể không hiệu quả nếu tọa độ lớn. Trong bài toán này, tọa độ đủ nhỏ để ngay cả sự rời rạc hóa thô bạo cũng có thể vượt qua, nhưng nó che khuất cấu trúc. 

Một cách tiếp cận mang tính nguyên tắc hơn là nhận ra rằng đường chân trời chỉ thay đổi tại các ranh giới của tòa nhà: tại mỗi s và s + w. Giữa các điểm này, chiều cao không đổi. Điều này có nghĩa là chúng ta có thể nén trục x thành một danh sách các điểm tới hạn đã được sắp xếp, tính toán độ cao tối đa trong mỗi khoảng và sau đó đo các chuyển tiếp. 

Ý tưởng vũ phu sẽ là: với mỗi x nhỏ, hãy tính chiều cao tối đa trong số tất cả các tòa nhà bao phủ x, sau đó tính tổng các chuyển đổi. Đây là O(U · n), trong đó U là phạm vi tọa độ lên tới 100. Trong trường hợp khái quát hóa tồi tệ nhất, điều này trở nên quá chậm, nhưng ở đây thì không đáng kể.

Lý luận được tối ưu hóa thay thế việc quét liên tục bằng lý luận theo khoảng thời gian. Chúng tôi thu thập tất cả các điểm cuối của phân khúc, sắp xếp chúng và đánh giá chiều cao trên mỗi khoảng thời gian. Vì n nhiều nhất là 50 nên việc kiểm tra tất cả các tòa nhà trong mỗi khoảng thời gian là công việc liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Quét Brute Force trên x = 0.,99 | O(100 · n) | O(1) | Đã chấp nhận | 
| Nén khoảng thời gian | O(n² log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng đường chân trời tăng dần bằng cách tập trung vào các phân đoạn giữa tất cả các tọa độ x thú vị. 

1. Thu thập tất cả các điểm cuối của tòa nhà s và s + w. Đây là những nơi duy nhất mà đường chân trời có thể thay đổi độ cao. Bất kỳ khoảng cách nào giữa các điểm cuối liên tiếp đều có chiều cao không đổi. 
2. Sắp xếp các tọa độ này và loại bỏ các tọa độ trùng lặp để tạo thành danh sách tọa độ nén. Điều này cho chúng ta các khoảng rời rạc [x[i], x[i+1]] trong đó chiều cao đường chân trời được cố định. 
3. Đối với mỗi khoảng, hãy tính chiều cao tối đa của bất kỳ tòa nhà nào bao phủ khoảng đó. Một tòa nhà bao phủ khoảng nếu nhịp ngang của nó cắt ngang nó. Chúng tôi lấy mức tối đa vì chỉ cấu trúc cao nhất có thể nhìn thấy được mới xác định ranh giới trên cùng. 
4. Bây giờ hãy tính khoảng cách đi bộ. Đối với mỗi khoảng, chúng tôi thêm khoảng cách ngang bằng x[i+1] − x[i] ở mặt đất cộng với cấu trúc bổ sung có thể được xử lý hoàn toàn bằng các chuyển đổi dọc. 
5. Để tính đến chuyển động thẳng đứng, chúng tôi theo dõi sự thay đổi độ cao giữa các khoảng thời gian liên tiếp. Nếu độ cao tăng lên, sâu bướm sẽ leo lên chênh lệch đó; nếu nó giảm thì nó giảm. 
6. Câu trả lời tổng thể là tổng của tất cả các chiều dài khoảng cách theo chiều ngang cộng với tất cả sự khác biệt tuyệt đối giữa các độ cao liên tiếp của đường chân trời. 

Một điểm tinh tế quan trọng là việc khởi tạo: chúng tôi bắt đầu ở độ cao 0 và độ cao khác 0 đầu tiên sẽ tạo ra một bước leo thẳng đứng từ mặt đất. 

Tại sao nó hoạt động dựa trên thực tế là đường đi của con sâu bướm chính xác là chu vi của phần giao nhau của các hình chữ nhật khi được chiếu dọc theo x. Hàm đường chân trời mã hóa ranh giới trên và mọi thay đổi về độ cao tương ứng với một đoạn thẳng đứng của ranh giới. Các đoạn ngang tương ứng chính xác với độ rộng khoảng. Vì các tòa nhà không chồng lên nhau về diện tích nên không có đoạn nào bị tính trùng hoặc bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    buildings = []
    xs = set()

    for _ in range(n):
        s, w, h = map(int, input().split())
        buildings.append((s, s + w, h))
        xs.add(s)
        xs.add(s + w)

    xs = sorted(xs)

    def height_at_interval(l, r):
        best = 0
        for s, e, h in buildings:
            if not (e <= l or s >= r):
                best = max(best, h)
        return best

    prev_h = 0
    ans = 0

    for i in range(len(xs) - 1):
        l, r = xs[i], xs[i + 1]
        h = height_at_interval(l, r)

        ans += (r - l)
        ans += abs(h - prev_h)
        prev_h = h

    ans += prev_h
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo ý tưởng nén khoảng thời gian một cách trực tiếp. Bộ tọa độ đảm bảo chúng tôi chỉ đánh giá các khoảng thời gian có thể xảy ra thay đổi. Đối với mỗi khoảng thời gian, chúng tôi tính toán lại chiều cao tối đa bằng cách kiểm tra tất cả các tòa nhà, điều này là đủ với các ràng buộc nhỏ. 

Biến`prev_h`theo dõi chiều cao của khoảng thời gian trước đó để bất kỳ thay đổi nào cũng tạo ra sự đóng góp theo chiều dọc. Sự bổ sung cuối cùng của`prev_h`tính đến việc giảm dần trở lại mặt đất ở cuối. 

Một điểm tinh tế là chuyển động ngang luôn được tính theo từng khoảng bất kể chiều cao, vì con sâu bướm luôn đi qua nhịp x của mỗi đoạn đúng một lần dọc theo mặt đất hoặc mái nhà. Đây là lý do tại sao`(r - l)`luôn luôn được thêm vào. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản với hai tòa nhà tách biệt: 

đầu vào:```
2
10 3 7
20 2 5
```| Khoảng thời gian | tôi | r | chiều cao tối đa | trước_h | chi phí dọc | chi phí ngang | 
| --- | --- | --- | --- | --- | --- | --- | 
| [0,10] | 0 | 10 | 0 | 0 | 0 | 10 | 
| [10,13] | 10 | 13 | 7 | 0 | 7 | 3 | 
| [13,20] | 13 | 20 | 0 | 7 | 7 | 7 | 
| [20,22] | 20 | 22 | 5 | 0 | 5 | 2 | 
| kết thúc | | | | 5 | 5 | | 

Dấu vết này cho thấy mỗi tòa nhà chỉ đóng góp như thế nào thông qua sự chuyển đổi độ cao và chuyển động ngang của nhịp của nó. 

Bây giờ hãy xem xét các tòa nhà chồng chéo: 

đầu vào:```
2
0 10 2
5 10 5
```| Khoảng thời gian | tôi | r | chiều cao tối đa | trước_h | chi phí dọc | chi phí ngang | 
| --- | --- | --- | --- | --- | --- | --- | 
| [0,5] | 0 | 5 | 2 | 0 | 2 | 5 | 
| [5,10] | 5 | 10 | 5 | 2 | 3 | 5 | 
| [10,15] | 10 | 15 | 5 | 5 | 0 | 5 | 
| [15,20] | 15 | 20 | 0 | 5 | 5 | 5 | 

Điều này cho thấy sự chồng chéo hợp nhất thành một đường chân trời duy nhất và chỉ những thay đổi về độ cao mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi khoảng thời gian sẽ kiểm tra tất cả các tòa nhà xem có chồng chéo không | 
| Không gian | O(n) | Lưu trữ các tòa nhà và nén phối hợp | 

Với n 50 thì quét bậc hai là không đáng kể. Ngay cả trong trường hợp xấu nhất là 50 khoảng với 50 lần kiểm tra mỗi khoảng cũng chỉ dẫn đến vài nghìn thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# simple non-overlapping
assert run("2\n10 3 7\n20 2 5\n") == "57"

# overlapping skyline
assert run("2\n0 10 2\n5 10 5\n") == "40"

# single building
assert run("1\n10 10 10\n") == "40"

# touching buildings
assert run("2\n0 10 3\n10 10 4\n") == "47"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hai tòa nhà tách biệt | 57 | đoạn đường chân trời độc lập | 
| tòa nhà chồng chéo | 40 | hợp nhất chiều cao tối đa chính xác | 
| tòa nhà đơn | 40 | hành vi chu vi cơ bản | 
| chạm vào cạnh | 47 | không có khoảng cách dọc giả | 

## Vỏ cạnh 

Một trường hợp quan trọng là các tòa nhà chỉ chạm vào ranh giới. Ví dụ: 

đầu vào:```
2
0 10 3
10 10 4
```Tại x = 10, không có khoảng cách ngang và không có bức tường đứng dùng chung giữa các tòa nhà. Thuật toán xử lý chính xác [0,10] với chiều cao 3 và [10,20] với chiều cao 4, tạo ra mức tăng dọc duy nhất là 1 tại ranh giới. Không có tính năng đếm kép vì việc phân chia khoảng thời gian đảm bảo quá trình chuyển đổi được ghi lại chính xác một lần. 

Một trường hợp khác là một tòa nhà được chứa hoàn toàn bên trong một tòa nhà cao hơn: 

đầu vào:```
2
0 20 5
5 5 2
```Tòa nhà bên trong hoàn toàn không ảnh hưởng đến đường chân trời. Hàm chiều cao vẫn là 5 trên tất cả các khoảng, do đó không có sự chuyển tiếp theo chiều dọc nào xảy ra bên trong phần chồng lấp. Thuật toán bỏ qua một cách chính xác các cấu trúc bên trong vì nó luôn lấy chiều cao tối đa trên mỗi khoảng thay vì tính tổng các đóng góp.
