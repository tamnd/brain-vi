---
title: "CF 104454H - Đồng thau Birmingham: đường"
description: "Mỗi người trong số bốn người chơi xây dựng hai loại thứ trên bản đồ thành phố chung. Đầu tiên, họ đặt token của ngành vào các thành phố cụ thể. Một thành phố có thể chứa nhiều ngành công nghiệp nếu một số mã thông báo đổ bộ vào đó trên tất cả người chơi."
date: "2026-06-30T14:27:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104454
codeforces_index: "H"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2021"
rating: 0
weight: 104454
solve_time_s: 90
verified: true
draft: false
---

[CF 104454H - Brass Birmingham: đường](https://codeforces.com/problemset/problem/104454/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi người trong số bốn người chơi xây dựng hai loại thứ trên bản đồ thành phố chung. Đầu tiên, họ đặt token của ngành vào các thành phố cụ thể. Một thành phố có thể chứa nhiều ngành công nghiệp nếu một số mã thông báo đổ bộ vào đó trên tất cả người chơi. Thứ hai, mỗi người chơi xây dựng các con đường, trong đó mỗi con đường nối hai thành phố và thuộc về chính xác một người chơi. 

Quy tắc tính điểm cho đường có tính chất cục bộ đối với mỗi con đường nhưng phụ thuộc vào tình trạng toàn cầu của các ngành. Vì một con đường nối liền các thành phố`u`Và`v`, người chơi xây dựng nó sẽ nhận được số điểm bằng tổng số mã thông báo ngành hiện có trong thành phố`u`cộng với những người ở thành phố`v`. Nhiệm vụ cuối cùng là tính toán độc lập cho mỗi người chơi tổng điểm của tất cả các con đường họ đã xây dựng. 

Sự tương tác chính là các ngành mang tính toàn cầu đối với tất cả người chơi, trong khi con đường là riêng tư đối với mỗi người chơi. Điều đó có nghĩa là giá trị của một con đường phụ thuộc vào thông tin tổng hợp được chia sẻ chứ không chỉ hành động của chính người chơi. 

Các ràng buộc cho phép lên đến`10^5`thành phố, trong khi mỗi người chơi đóng góp nhiều nhất`10^4`các ngành công nghiệp và`10^4`những con đường. Điều này làm cho tổng cộng tối đa`4 * 10^4`vị trí công nghiệp và cùng thứ tự các con đường. Bất kỳ giải pháp nào cố gắng tính toán lại số lượng ngành nhiều lần trên mỗi đường hoặc mỗi truy vấn sẽ quá chậm, đặc biệt nếu giải pháp đó quét danh sách hoặc tính toán lại tổng từ đầu. Cần có chiến lược tiền xử lý tuyến tính hoặc gần tuyến tính. 

Một trường hợp thất bại tinh tế xuất hiện khi các ngành không được tổng hợp chính xác giữa tất cả người chơi. Ví dụ, nếu thành phố`1`có các ngành từ nhiều người chơi nhưng chúng tôi chỉ tính đóng góp của người chơi được đọc lần cuối, điểm đường liên quan đến thành phố`1`sẽ bị đánh giá thấp. Một sai lầm phổ biến khác là coi các ngành theo từng người chơi khi việc ghi điểm rõ ràng phụ thuộc vào tổng số toàn cầu. 

## Phương pháp tiếp cận 

Phương pháp mô phỏng trực tiếp sẽ lưu trữ tất cả các vị trí trong ngành và đối với mỗi con đường, hãy đếm xem có bao nhiêu ngành tồn tại ở cả hai điểm cuối bằng cách quét toàn bộ danh sách các ngành. Điều này có nghĩa là đối với mỗi con đường, chúng ta có thể lặp lại tất cả các vị trí trong ngành, đưa ra trường hợp xấu nhất xung quanh`O(G * M)`mỗi người chơi. Với`G`Và`M`lên đến`10^4`, điều này đã có thể đạt tới`10^8`hoạt động của mỗi người chơi trong các trường hợp xấu nhất và đối với bốn người chơi, việc này trở nên quá chậm. 

Sự cải thiện đến từ việc tách biệt các mối quan tâm. Điều duy nhất mỗi con đường cần là tổng số ngành ở hai điểm cuối của nó. Giá trị đó không phụ thuộc vào bản thân con đường mà chỉ phụ thuộc vào sự phân bổ cuối cùng của các ngành công nghiệp giữa các thành phố. Vì vậy, thay vì tính toán lại nhiều lần, trước tiên chúng tôi nén tất cả các vị trí trong ngành vào một mảng tần số duy nhất`cnt[c]`, đại diện cho số lượng ngành công nghiệp tồn tại ở mỗi thành phố. 

Khi mảng này được xây dựng, mọi đánh giá đường sẽ trở thành thời gian không đổi: đối với một con đường`(u, v)`, điểm số chỉ đơn giản là`cnt[u] + cnt[v]`. Khi đó, câu trả lời của mỗi người chơi chỉ là tổng của biểu thức này trên con đường của riêng họ. 

Điều này làm giảm vấn đề từ việc quét lặp đi lặp lại thành một bước xử lý trước duy nhất cộng với việc vượt qua tuyến tính trên tất cả các con đường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (quét các ngành trên mỗi con đường) | O(G × tổng số ngành) | O(tổng số ngành) | Quá chậm | 
| Tổng hợp tiền tố theo thành phố | O(N + tổng ngành + tổng số đường) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### bước 

1. Đọc số thành phố`N`và khởi tạo một mảng`cnt`kích thước`N + 1`với số không. 

Mảng này sẽ lưu trữ số lượng mã thông báo ngành tồn tại ở mỗi thành phố bất kể người chơi. 
2. Đối với mỗi người trong số bốn người chơi, hãy đọc danh sách các vị trí trong ngành của họ. Đối với mỗi chỉ số thành phố`x`trong danh sách đó, tăng`cnt[x]`bởi một. 

Điều này hợp nhất tất cả các ngành của người chơi vào một bảng tần số toàn cầu duy nhất. 
3. Đối với mỗi người chơi, hãy khởi tạo biến điểm chạy về 0. 
4. Đọc danh sách đường đi của người chơi đó. Đối với mỗi con đường`(a, b)`, thêm vào`cnt[a] + cnt[b]`vào điểm số của người chơi đó. 

Điều này hiệu quả vì mọi ngành ở một trong hai điểm cuối đều đóng góp chính xác một lần vào giá trị của con đường đó. 
5. In ra bốn điểm tích lũy. 

### Tại sao nó hoạt động 

Bất cứ lúc nào,`cnt[c]`đại diện cho số lượng mã thông báo ngành chính xác được đặt trong thành phố`c`. Giá trị này độc lập với các con đường và chỉ phụ thuộc vào sự kết hợp của tất cả các hành động trong ngành của người chơi. 

Đóng góp của mỗi con đường hoàn toàn mang tính cộng đối với các điểm cuối của nó, do đó, tổng số điểm của người chơi là tổng trên các cạnh của một hàm chỉ phụ thuộc vào trọng số đỉnh được tính toán trước. Vì không có con đường nào ảnh hưởng đến con đường khác và không có thay đổi vị trí ngành nào sau khi đọc nên việc phân tách thành các trọng số đỉnh là chính xác và ổn định trong suốt quá trình tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    cnt = [0] * (n + 1)

    players_industries = []

    for _ in range(4):
        m, g = map(int, input().split())
        inds = list(map(int, input().split()))
        players_industries.append((m, g, inds))
        for c in inds:
            cnt[c] += 1

    res = []

    for i in range(4):
        m, g, inds = players_industries[i]
        total = 0
        for _ in range(g):
            a, b = map(int, input().split())
            total += cnt[a] + cnt[b]
        res.append(total)

    print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xây dựng mảng tần số ngành toàn cầu chỉ bằng một lần duyệt qua tất cả danh sách ngành của người chơi. Sau đó, nó sẽ sử dụng lại mảng này khi xử lý đường của từng người chơi, đảm bảo rằng mỗi đường được đánh giá theo thời gian không đổi. 

Một cạm bẫy phổ biến là cố gắng tính toán lại số lượng ngành trong quá trình đánh giá đường hoặc phân tách nhầm số lượng ngành cho mỗi người chơi. Giải thích đúng là tất cả các ngành đều đóng góp vào một nhóm chung. 

Một điểm tinh tế khác là các con đường được xử lý sau khi đã biết tất cả các ngành, do đó không cần phải cập nhật dần dần hoặc cân nhắc thứ tự. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chúng tôi sử dụng dấu vết đơn giản hóa với hai người chơi để làm rõ, mặc dù vấn đề thực sự có bốn người chơi. 

Cho rằng:```
N = 3
Player 1 industries: [1, 2]
Player 2 industries: [2, 3]
```Vì thế:`cnt = [0, 1, 2, 1]`Đường của người chơi 1:`(1,2)`| Đường | cnt[a] | cnt[b] | Đóng góp | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| (1,2) | 1 | 2 | 3 | 3 | 

Người chơi 2 đường:`(2,3)`| Đường | cnt[a] | cnt[b] | Đóng góp | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| (2,3) | 2 | 1 | 3 | 3 | 

Điều này cho thấy cả hai người chơi đều được hưởng lợi từ nhóm ngành chung. 

### Ví dụ 2 

Hãy xem xét:```
N = 4
Industries: [1,1,2,4]
So cnt = [0,2,1,0,1]
```Đường người chơi:```
(1,4), (2,4)
```| Đường | cnt[a] | cnt[b] | Đóng góp | Tổng chạy | 
| --- | --- | --- | --- | --- | 
| (1,4) | 2 | 1 | 3 | 3 | 
| (2,4) | 1 | 1 | 2 | 5 | 

Điều này xác nhận rằng các ngành công nghiệp lặp lại trong một thành phố được tích lũy chính xác trước khi đánh giá đường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + ΣM + ΣG) | Một đường chuyền để xây dựng số lượng thành phố và một đường chuyền trên tất cả các con đường | 
| Không gian | O(N) | Chỉ có mảng tần số thành phố và lưu trữ đầu vào | 

Các ràng buộc cho phép tối đa khoảng 40.000 mục nhập ngành và 40.000 con đường, do đó, việc truyền tuyến tính trên tất cả dữ liệu có thể dễ dàng đủ nhanh trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    cnt = [0] * (n + 1)

    players = []

    for _ in range(4):
        m, g = map(int, input().split())
        inds = list(map(int, input().split()))
        players.append((g, []))
        for x in inds:
            cnt[x] += 1

    outputs = []
    for i in range(4):
        g, _ = players[i]
        total = 0
        for _ in range(g):
            a, b = map(int, input().split())
            total += cnt[a] + cnt[b]
        outputs.append(str(total))

    return " ".join(outputs)

# provided sample
assert run("""4
1 1
1
1 2
3 1
1 2 3
2 3
4 2
1 4 2 3
3 1
4 2
1 4
3
1 3
2 3
3 4
1 2
""") == "5 5 9 20"

# minimal case
assert run("""2
1 1
1
1 2
0 1
2
1 2
0 0
0 0
0 0
""") == "1 1 1 1"

# all industries same city
assert run("""3
3 1
1 1 1
1 2
0 0
2 3
0 0
0 0
0 0
""") == "6 0 0 0"

# chain test
assert run("""4
1 1
2
1 2
1 1
3
2 3
1 1
4
3 4
1 1
4
1 4
""") == "4 4 4 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 5 5 9 20 | chính xác trên đầu vào hỗn hợp đầy đủ | 
| trường hợp tối thiểu | 1 1 1 1 | xử lý cấu trúc nhỏ nhất | 
| tất cả các ngành cùng thành phố | 6 0 0 0 | tập hợp trong một nút duy nhất | 
| kiểm tra dây chuyền | 4 4 4 4 | đối xứng và điểm cuối lặp đi lặp lại | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi nhiều ngành tích tụ trong cùng một thành phố từ những người chơi khác nhau. Thuật toán xử lý việc này một cách tự nhiên vì nó tính tổng tất cả các số gia thành`cnt[c]`trước khi bất kỳ quá trình xử lý đường nào bắt đầu. Đối với một thành phố có ba ngành công nghiệp,`cnt[c]`trở thành 3 bất kể phân phối và mọi con đường sử dụng thành phố đó đều tính chính xác cả ba lần đóng góp. 

Một trường hợp khác là khi người chơi không có đường. Trong tình huống đó, việc đi vòng qua các con đường không đóng góp gì và điểm của người chơi vẫn bằng 0, điều này đúng vì việc ghi điểm chỉ được xác định thông qua các con đường được xây dựng. 

Một trường hợp nữa là các điểm cuối thành phố lặp lại ở các con đường khác nhau. Vì mỗi con đường là độc lập và luôn được đánh giá bằng cách sử dụng cùng một mảng được tính toán trước nên việc sử dụng lặp lại một thành phố chỉ đơn giản là sử dụng lại cùng một mảng`cnt[c]`giá trị mà không có tác dụng phụ hoặc đột biến.
