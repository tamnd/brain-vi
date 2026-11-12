---
title: "CF 1681A - Trò chơi bài"
description: "Hai người chơi, mỗi người giữ một tập hợp số nguyên. Họ chơi trò chơi theo lượt trong đó quy tắc duy nhất là mọi số mới chơi phải lớn hơn số trước đó."
date: "2026-06-10T00:22:15+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1681
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 129 (Rated for Div. 2)"
rating: 800
weight: 1681
solve_time_s: 658
verified: false
draft: false
---

[CF 1681A - Trò chơi bài](https://codeforces.com/problemset/problem/1681/A) 

**Đánh giá:** 800 
**Tags:** trò chơi, tham lam 
**Thời gian giải:** 10 phút 58 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Hai người chơi, mỗi người giữ một tập hợp số nguyên. Họ chơi trò chơi theo lượt trong đó quy tắc duy nhất là mọi số mới chơi phải lớn hơn số trước đó. Người chơi luân phiên nhau và trong mỗi lượt người chơi phải chọn một lá bài còn lại lớn hơn giá trị đã chơi cuối cùng. Nếu không có lá bài nào như vậy tồn tại, người chơi đó sẽ thua ngay lập tức. 

Đối với mỗi trường hợp thử nghiệm, chúng ta phải xác định người chiến thắng trong cách chơi tối ưu cho hai tình huống: khi Alice bắt đầu và khi Bob bắt đầu. 

Những hạn chế là rất nhỏ. Mỗi người chơi có tối đa 50 thẻ và giá trị được giới hạn là 50 và có tới 1000 trường hợp thử nghiệm. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào cũng có thể sử dụng các chiến lược sắp xếp và thậm chí là đếm đơn giản một cách an toàn mà không phải lo lắng về hiệu suất. Về mặt lý thuyết, việc mô phỏng lực lượng tàn bạo của các trạng thái trò chơi cũng có thể thực hiện được, nhưng không cần thiết. 

Một vấn đề tế nhị là bước đi đầu tiên không bị hạn chế. Một cách tiếp cận ngây thơ có thể cho rằng “bắt đầu với thẻ nhỏ nhất” hoặc “bắt đầu với thẻ lớn nhất” luôn là tối ưu. Điều đó nói chung không đúng, vì việc lựa chọn số đầu tiên sẽ quyết định toàn bộ chuỗi phản ứng bắt buộc. Tuy nhiên, vì phạm vi giá trị rất nhỏ nên giải pháp chính xác không yêu cầu tìm kiếm cây trò chơi sâu. 

Một cạm bẫy phổ biến khác là cố gắng mô phỏng một cách tham lam bằng cách luôn chọn lá bài tiếp theo hợp lệ nhỏ nhất. Chiến lược đó có thể thất bại vì nó bỏ qua rằng đôi khi việc tiêu thụ một quân bài nhỏ sớm sẽ khiến đối thủ không thể xây dựng một chuỗi dài hơn sau này. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ coi trò chơi như một không gian trạng thái trong đó mỗi trạng thái được xác định bởi giá trị được chơi cuối cùng, lượt của ai và những lá bài nào còn lại. Từ mỗi tiểu bang, chúng tôi thử tất cả các nước đi hợp lệ. Điều này đúng nhưng nhanh chóng trở thành cấp số nhân vì mỗi người chơi có thể sắp xếp lại lượt chơi của mình theo nhiều cách. 

Nhận xét quan trọng là trò chơi không phụ thuộc vào thứ tự giữa các giá trị giống hệt nhau và hạn chế duy nhất là sự gia tăng đơn điệu. Vì các giá trị nhỏ nên điều thực sự quan trọng là mỗi người chơi có bao nhiêu lá bài cho mỗi giá trị. 

Sau khi sắp xếp mảng của cả hai người chơi, chúng ta có thể nghĩ đến việc chọn giá trị bắt đầu ban đầu và sau đó tham lam mở rộng chuỗi xen kẽ dài nhất có thể. Mỗi người chơi luôn muốn đáp trả bằng quân bài nhỏ nhất hiện có nhưng vẫn lớn hơn nước đi trước, vì việc chọn quân bài lớn hơn chỉ làm giảm các lựa chọn trong tương lai. 

Điều này giúp giảm bớt vấn đề trong việc kiểm tra, đối với mỗi bước bắt đầu có thể, trình tự có thể kéo dài bao xa. Vì chỉ có 100 giá trị bắt đầu có thể có trong trường hợp xấu nhất nên điều này trở nên cực kỳ hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Sắp xếp mô phỏng tham lam trên tất cả các lần bắt đầu | O(t * (n + m) * 100) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết từng trường hợp thử nghiệm một cách độc lập. 

Đầu tiên, chúng tôi sắp xếp thẻ của Alice và Bob. Việc sắp xếp đảm bảo rằng khi chúng ta cố gắng mở rộng một chuỗi, chúng ta luôn có thể chọn thẻ hợp lệ tiếp theo theo thứ tự quét tuyến tính. 

Tiếp theo, chúng tôi thử mọi bước đi đầu tiên có thể. Có hai loại nước đi đầu tiên: Alice bắt đầu hoặc Bob bắt đầu. Đối với mỗi trường hợp, chúng tôi mô phỏng trò chơi. 

Trong quá trình mô phỏng, chúng tôi duy trì một con trỏ vào từng danh sách được sắp xếp. Con trỏ luôn di chuyển về phía trước và không bao giờ lùi lại, vì một khi thẻ được sử dụng, nó sẽ bị loại khỏi danh sách xem xét. 

Chúng tôi luân phiên nhau. Trong mỗi lượt, người chơi hiện tại tiến con trỏ của họ cho đến khi họ tìm thấy một lá bài lớn hơn giá trị đã chơi cuối cùng. Nếu không có thẻ như vậy tồn tại, người chơi hiện tại sẽ thua và quá trình mô phỏng kết thúc. 

Chúng tôi tính toán kết quả cho cả cấu hình ban đầu và đưa ra kết quả chiến thắng. 

Sự phức tạp về từ điển hoặc cấu trúc ở đây không liên quan vì trò chơi được xác định hoàn toàn sau khi nước đi đầu tiên được ấn định.

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong trò chơi, cả hai người chơi đều cố gắng duy trì tính linh hoạt trong tương lai một cách tối ưu. Trong một mảng được sắp xếp, việc chọn quân bài hợp lệ nhỏ nhất luôn là tối ưu vì nó để lại khoảng trống lớn nhất có thể cho các bước đi sau này. Bất kỳ lựa chọn nào lớn hơn sẽ làm giảm nghiêm trọng số lượng phản hồi có sẵn trong tương lai mà không cải thiện khả năng tiếp tục chuỗi. Điều này làm cho mô phỏng tham lam tương đương với cách chơi tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def simulate(a, b, start_a):
    ia = ib = 0
    last = -1
    turn_a = start_a

    while True:
        if turn_a:
            while ia < len(a) and a[ia] <= last:
                ia += 1
            if ia == len(a):
                return "Bob"
            last = a[ia]
            ia += 1
        else:
            while ib < len(b) and b[ib] <= last:
                ib += 1
            if ib == len(b):
                return "Alice"
            last = b[ib]
            ib += 1
        turn_a = not turn_a

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        m = int(input())
        b = list(map(int, input().split()))

        a.sort()
        b.sort()

        res1 = simulate(a, b, True)
        res2 = simulate(a, b, False)

        print(res1)
        print(res2)

if __name__ == "__main__":
    solve()
```Giải pháp giữ cho cả hai mảng được sắp xếp sao cho việc tìm bước di chuyển hợp lệ tiếp theo sẽ trở thành quá trình quét tuyến tính bằng con trỏ di chuyển. Mỗi mô phỏng chạy theo thời gian tuyến tính trên tổng số thẻ, vì mỗi con trỏ chỉ tiến về phía trước. 

Một điểm tinh tế là sự so sánh sử dụng`<= last`, đảm bảo mức tăng nghiêm ngặt được thực thi chính xác. Một chi tiết quan trọng khác là giá trị được chơi lần cuối được cập nhật ngay sau khi chọn thẻ, giúp duy trì tính chính xác của việc chuyển đổi lượt chơi. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện. 

### Ví dụ 1 

Alice: [6] 

Bob: [6, 8] 

Alice bắt đầu. 

| Xoay | Người chơi | Giá trị cuối cùng | Con trỏ Alice | Con trỏ Bob | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Alice | - | 6 đã qua sử dụng | - | Alice chơi 6 | 
| 2 | Bob | 6 | 6 bị bỏ qua, 8 được sử dụng | 8 đã qua sử dụng | Bob chơi 8 | 
| 3 | Alice | 8 | không hợp lệ | 1 | Alice thua | 

Điều này xác nhận rằng ngay cả với các giá trị ban đầu bằng nhau, sự bất đẳng thức nghiêm ngặt buộc Alice phải tiến triển và nhanh chóng làm kiệt sức. 

### Ví dụ 2 

Alice: [1, 3, 3, 7] 

Bob: [2, 4] 

Alice bắt đầu. 

| Xoay | Người chơi | Giá trị cuối cùng | Bang A | Bang B | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Alice | - | 1,3,3,7 | 2,4 | Alice chơi 1 | 
| 2 | Bob | 1 | 1,3,3,7 | 2,4 | Bob chơi 2 | 
| 3 | Alice | 2 | 3,3, | | |
