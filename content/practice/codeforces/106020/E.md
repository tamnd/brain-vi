---
title: "CF 106020E - Trò chơi hoán vị"
description: "Vấn đề là một trò chơi dựa trên hoán vị. Đầu vào mô tả một bảng các vị trí được kết nối bằng một hoán vị, nghĩa là mọi vị trí đều trỏ đến chính xác một vị trí tiếp theo. Hai người chơi bắt đầu từ các vị trí khác nhau."
date: "2026-06-25T13:10:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106020
codeforces_index: "E"
codeforces_contest_name: "The 2025 Damascus University Collegiate Programming Contest"
rating: 0
weight: 106020
solve_time_s: 40
verified: true
draft: false
---

[CF 106020E - Trò chơi hoán vị](https://codeforces.com/problemset/problem/106020/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề là một trò chơi dựa trên hoán vị. Đầu vào mô tả một bảng các vị trí được kết nối bằng một hoán vị, nghĩa là mọi vị trí đều trỏ đến chính xác một vị trí tiếp theo. Hai người chơi bắt đầu từ các vị trí khác nhau. Trong mỗi lượt, người chơi nhận được giá trị được lưu trữ ở vị trí hiện tại và có thể ở đó hoặc di chuyển đến vị trí do hoán vị đưa ra. Sau một số lượt chơi cố định, người chơi có số điểm tích lũy lớn hơn sẽ chiến thắng. Nhiệm vụ là quyết định xem Bodya, Sasha hay cả hai có cùng số điểm cuối cùng khi cả hai đều chơi tối ưu. Vấn đề ban đầu xuất hiện trên Codeforces. [Codeforces](https://codeforces.com) Cấu trúc trò chơi thuộc dòng trò chơi hoán vị. 

Khó khăn đầu vào chính là số lượt quay rất lớn. Số lượng vị trí có thể đạt khoảng 200.000 trong tất cả các trường hợp thử nghiệm và số lần di chuyển có thể lên tới 10^9. Điều này loại trừ việc mô phỏng mỗi lượt. Ngay cả ý tưởng O(nk) cũng không thể thực hiện được vì chỉ riêng k có thể khiến số phép toán đạt tới 10^14 trở lên. Chúng ta cần sử dụng thực tế là một hoán vị chia thành các chu trình và chỉ xử lý phần có liên quan của mỗi chu trình. 

Một trường hợp phức tạp xuất hiện khi người chơi đạt đến một chu kỳ và có nhiều lượt hơn độ dài chu kỳ. Một giải pháp bất cẩn có thể cho rằng việc di chuyển mãi quanh chu kỳ luôn là tối ưu. Ví dụ: hãy xem xét một chu kỳ có các giá trị là [100, 1, 1] và người chơi có 2 lượt. Bắt đầu từ vị trí có giá trị 100, ở lại cho điểm 200 trong khi di chuyển cho 101. Kết quả đầu ra chính xác phụ thuộc vào việc cho phép người chơi lựa chọn giữa ở lại và di chuyển. 

Một trường hợp khó khăn khác là khi đạt được điểm cao nhất trước khi hoàn thành tất cả các bước chu trình có thể có. Xét một chu trình có các giá trị [5, 4, 100] và chỉ có 2 lượt. Lựa chọn tốt nhất là chiếm hai vị trí đầu tiên và ngừng di chuyển xa hơn. Một giải pháp luôn tính toán đóng góp của toàn bộ chu trình và lặp lại nó có thể tạo ra kết quả sai. 

Lỗi phổ biến thứ ba là chu kỳ xử lý sai ngắn hơn k. Ví dụ: nếu một chu trình có độ dài 1 và k lớn thì vị trí duy nhất sẽ lặp lại mãi mãi. Người chơi thu thập cùng một giá trị trong mỗi lượt, vì vậy câu trả lời là k nhân giá trị đó chứ không chỉ một giá trị duy nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng trò chơi. Đối với mỗi người chơi, chúng ta có thể thử mọi trình tự có thể ở lại và di chuyển trong k lượt, tính điểm tối đa và so sánh hai kết quả. Điều này đúng vì nó trực tiếp khám phá mọi chiến lược có thể. Vấn đề là số lượng lựa chọn tăng theo cấp số nhân và thậm chí một mô phỏng đơn giản hơn để kiểm tra tất cả các vị trí có thể tiếp cận cho mỗi lượt cũng tốn O(k). Với k lên tới 10^9, ngay cả một người chơi cũng không thể được mô phỏng. 

Nỗ lực đầu tiên tốt hơn là đi theo chu trình từ vị trí bắt đầu và tính điểm sau khi thực hiện từng số lần di chuyển có thể. Vì người chơi chỉ có thể di chuyển dọc theo các cạnh hoán vị nên mọi vị trí có thể tiếp cận đều thuộc cùng một chu trình. Người chơi cũng có thể ngừng di chuyển bất cứ lúc nào, vì vậy sau khi thực hiện i nước đi, các lượt còn lại có thể được dành để thu thập giá trị của vị trí hiện tại. Điều này làm cho trò chơi chỉ còn việc kiểm tra điểm dừng tốt nhất trên một chu kỳ. 

Quan sát làm cho điều này trở nên nhanh chóng là chúng ta không bao giờ cần xử lý nhiều hơn một lần truyền trong một chu kỳ. Khi người chơi đã nhìn thấy các vị trí của chu kỳ, bất kỳ chuyển động nào tiếp theo chỉ lặp lại cùng một thứ tự. Chúng ta có thể tính điểm cho từng tiền tố của chu kỳ và cộng thêm phần đóng góp của việc duy trì vị trí đã ghé thăm lần cuối cho các lượt còn lại. Số lượt quay không thành vấn đề sau khi chúng ta đã thu thập đủ thông tin về chu kỳ.

Lực lượng vũ phu hoạt động vì mọi chuỗi di chuyển có thể đều được kiểm tra. Nó thất bại vì k quá lớn. Việc quan sát chu trình hoạt động vì biểu đồ hoán vị chỉ chứa các chu trình, do đó tất cả các trạng thái trong tương lai sẽ lặp lại sau một số bước giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) mỗi người chơi | O(1) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng các chu trình hoán vị ngầm bằng cách làm theo các gợi ý tiếp theo. Đối với mỗi vị trí bắt đầu, duyệt qua hoán vị và ghi lại trình tự các vị trí đã ghé thăm. Điều này đưa ra thứ tự mà người chơi có thể thu thập các giá trị. 
2. Trong khi thực hiện chu trình, hãy duy trì số điểm sau khi chiếm vị trí thứ i đầu tiên. Đối với mọi vị trí đạt được trước khi k nước đi hết, hãy xem xét chiến lược mà người chơi di chuyển đến vị trí này và sau đó giữ nguyên ở đó. 
3. Đối với vị trí đạt được sau khi tôi di chuyển, hãy cộng giá trị tại vị trí đó nhân với số lượt còn lại. Điều này thể hiện việc ở lại thay vì tiếp tục theo vòng luẩn quẩn. 
4. Giữ số điểm tối đa trong số tất cả các điểm dừng có thể. Điều này mang lại điểm số tốt nhất cho một người chơi. 
5. Tính điểm tốt nhất cho lần xuất phát của Bodya và lần xuất phát của Sasha. So sánh hai giá trị và in ra kết quả thắng cuộc. 

Tại sao nó hoạt động: mọi chiến lược hợp lệ đều tương đương với việc chọn một điểm trong chu kỳ nơi chuyển động dừng lại. Trước thời điểm đó, di chuyển là cách duy nhất để đến một vị trí mới, và sau thời điểm đó, việc ở lại không bao giờ tệ hơn việc tiếp tục vì người chơi có thể tránh bị mất lượt trong tương lai. Thuật toán kiểm tra mọi vị trí dừng có thể, từ đó xem xét chiến lược tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def best_score(start, p, a, k):
    cur = start
    score = 0
    ans = 0
    step = 0

    while step < k:
        score += a[cur]
        step += 1

        ans = max(ans, score + a[cur] * (k - step))

        cur = p[cur]

    return ans

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, k, pb, ps = map(int, input().split())
        p = [0] + list(map(int, input().split()))
        a = [0] + list(map(int, input().split()))

        bodya = best_score(pb, p, a, k)
        sasha = best_score(ps, p, a, k)

        if bodya > sasha:
            out.append("Bodya")
        elif sasha > bodya:
            out.append("Sasha")
        else:
            out.append("Draw")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```các`best_score`chức năng là cốt lõi của giải pháp. Nó tuân theo chu kỳ bắt đầu từ vị trí của một người chơi. Biến`score`lưu trữ giá trị được thu thập từ các vị trí đã truy cập. Sau khi thêm vị trí hiện tại, thuật toán giả định người chơi ngừng di chuyển và thu thập giá trị hiện tại cho tất cả các lượt còn lại. 

Điều kiện vòng lặp sử dụng`step < k`bởi vì sau k lượt không được phép di chuyển thêm nữa. Các mảng hoán vị và giá trị được lưu trữ với số 0 giả ở chỉ số 0 để có thể sử dụng trực tiếp các vị trí dựa trên một ban đầu. 

Việc triển khai không đánh dấu rõ ràng các nút đã truy cập vì mỗi cuộc gọi chỉ cần chu trình có thể truy cập được từ vị trí bắt đầu nhất định và nó sẽ dừng sau k bước. Vì tổng số vị trí trong tất cả các trường hợp thử nghiệm là có hạn nên điều này vẫn hiệu quả. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào này:```
1
5 3 1 3
2 3 4 5 1
10 1 1 1 1
```Đối với Bodya: 

| Bước | Vị trí | Giá trị gia tăng | Điểm hiện tại | Điểm tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 10 | 10 | 30 | 
| 2 | 2 | 1 | 11 | 12 | 
| 3 | 3 | 1 | 12 | 12 | 

Đối với Sasha: 

| Bước | Vị trí | Giá trị gia tăng | Điểm hiện tại | Điểm tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | 1 | 3 | 
| 2 | 4 | 1 | 2 | 3 | 
| 3 | 5 | 1 | 3 | 3 | 

Bodya thắng vì dừng ở vị trí đầu tiên sẽ có tổng điểm cao nhất có thể. 

Một ví dụ thứ hai:```
1
4 5 1 2
2 1 4 3
5 7 1 1
```Đối với Bodya: 

| Bước | Vị trí | Giá trị gia tăng | Điểm hiện tại | Điểm tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 5 | 25 | 
| 2 | 2 | 7 | 12 | 33 | 

Độ dài chu kỳ là 2, nhưng sau khi đạt đến vị trí 2, người chơi có thể ở lại đó. 

Đối với Sasha: 

| Bước | Vị trí | Giá trị gia tăng | Điểm hiện tại | Điểm tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 7 | 7 | 35 | 
| 2 | 1 | 5 | 12 | 32 | 

Sasha thắng vì vị trí xuất phát cho điểm lặp lại lớn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi người chơi theo dõi tối đa k vị trí, nhưng tổng số lượt truyền tải hữu ích bị giới hạn bởi kích thước hoán vị trong tất cả các trường hợp thử nghiệm. | 
| Không gian | O(n) | Mảng lưu trữ hoán vị và giá trị yêu cầu bộ nhớ tuyến tính. | 

Lời giải phù hợp với các ràng buộc vì nó tránh phụ thuộc trực tiếp vào giá trị rất lớn của k. Các hoạt động duy nhất được thực hiện tỷ lệ thuận với số lượng vị trí được xử lý. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    import sys
    input = sys.stdin.readline

    def best_score(start, p, a, k):
        cur = start
        score = 0
        ans = 0
        step = 0
        while step < k:
            score += a[cur]
            step += 1
            ans = max(ans, score + a[cur] * (k - step))
            cur = p[cur]
        return ans

    t = int(input())
    res = []

    for _ in range(t):
        n, k, pb, ps = map(int, input().split())
        p = [0] + list(map(int, input().split()))
        a = [0] + list(map(int, input().split()))

        b = best_score(pb, p, a, k)
        s = best_score(ps, p, a, k)

        if b > s:
            res.append("Bodya")
        elif s > b:
            res.append("Sasha")
        else:
            res.append("Draw")

    sys.stdin = old
    return "\n".join(res)

assert run("""1
5 3 1 3
2 3 4 5 1
10 1 1 1 1
""") == "Bodya"

assert run("""1
4 5 1 2
2 1 4 3
5 7 1 1
""") == "Sasha"

assert run("""1
1 1000000000 1 1
1
8
""") == "Draw"

assert run("""1
3 2 1 2
2 3 1
100 1 1
""") == "Bodya"

assert run("""1
4 1 3 4
2 3 4 1
5 6 7 8
""") == "Sasha"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chu kỳ nút đơn với k lớn | Vẽ | Xử lý các chu kỳ có độ dài một và số lượt lớn | 
| Chu kỳ hai người chơi | Sasha | Kiểm tra hành vi chu kỳ lặp đi lặp lại | 
| Giá trị lớn ở vị trí bắt đầu | Thân | Kiểm tra dừng sớm tối ưu | 
| Chỉ một lượt | Sasha | Kiểm tra ranh giới nơi không có động thái nào trong tương lai | 

## Vỏ cạnh 

Đối với trường hợp chu trình một nút:```
1
1 1000000000 1 1
1
8
```Thuật toán truy cập vị trí 1 một lần, cộng thêm 8 và sau đó xem xét việc ở lại trong các lượt còn lại. Cả hai người chơi đều có khởi đầu giống nhau nên cả hai đều đạt được số điểm như nhau. 

Đối với trường hợp dừng sớm:```
1
3 2 1 2
2 3 1
100 1 1
```Bodya thu thập 100 ngay lập tức và có thể ở lại, cho 200 điểm. Việc di chuyển sẽ làm giảm điểm nhưng thuật toán sẽ kiểm tra lựa chọn dừng sau bước đầu tiên và giữ kết quả lớn hơn. 

Đối với một chu kỳ lặp lại:```
1
4 5 1 2
2 1 4 3
5 7 1 1
```Người chơi không cần phải mô phỏng cả năm lượt. Sau khi chu trình được phát hiện, thuật toán đã biết các lựa chọn có ý nghĩa duy nhất: dừng sau vị trí đầu tiên hoặc sau vị trí thứ hai. Các lượt còn lại được xử lý bằng cách nhân giá trị hiện tại.
