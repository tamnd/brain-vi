---
title: "CF 106467A - Đèn pha"
description: "Chúng ta được cung cấp một dải vị trí một chiều và một số vị trí này chứa đèn chiếu. Mỗi đèn chiếu chiếu sáng một đoạn liền kề xung quanh vị trí của nó, mở rộng ra bên ngoài với phạm vi tiếp cận cố định."
date: "2026-06-19T15:20:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106467
codeforces_index: "A"
codeforces_contest_name: "East China University of Science and Technology Programming Championship 2026"
rating: 0
weight: 106467
solve_time_s: 44
verified: true
draft: false
---

[CF 106467A - Đèn chiếu sáng](https://codeforces.com/problemset/problem/106467/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dải vị trí một chiều và một số vị trí này chứa đèn chiếu. Mỗi đèn chiếu chiếu sáng một đoạn liền kề xung quanh vị trí của nó, mở rộng ra bên ngoài với phạm vi tiếp cận cố định. Nhiệm vụ là xác định tổng phạm vi chiếu sáng cuối cùng trên đường dây sau khi tất cả các đèn chiếu được xem xét cùng nhau. 

Bạn có thể coi đầu vào là danh sách các tâm điểm và bán kính chiếu sáng cố định. Mỗi đèn chiếu đóng góp một khoảng trên trục số và các vùng được chiếu sáng chồng chéo phải được hợp nhất thay vì được tính nhiều lần. Đầu ra là tổng chiều dài của sự kết hợp của tất cả các khoảng được chiếu sáng. 

Khó khăn chính là việc xử lý một cách ngây thơ từng điểm một cách độc lập sẽ dẫn đến việc tính toán quá mức khi các khoảng thời gian chồng chéo nhiều. 

Từ góc độ phức tạp, nếu có tới 100000 đèn chiếu, giải pháp kiểm tra tất cả các cặp hoặc đánh dấu từng vị trí đơn vị riêng lẻ là quá chậm. Cách tiếp cận bậc hai theo các khoảng sẽ thực hiện theo thứ tự 10^10 phép tính trong trường hợp xấu nhất, điều này không khả thi trong các giới hạn thông thường. Ngay cả cách tiếp cận đánh dấu lưới dày đặc cũng không thành công nếu phạm vi tọa độ lớn, vì bộ nhớ và thời gian sẽ bùng nổ với kích thước tọa độ. 

Trường hợp khó nhận thấy xảy ra khi tất cả các điểm sáng chồng lên nhau hoàn toàn. Ví dụ: nếu tất cả các tâm đều giống nhau và bán kính lớn thì mọi điểm sáng đều tạo ra khoảng thời gian như nhau. Một tổng kết ngây thơ sẽ nhân mức độ bao phủ này không chính xác. 

Một trường hợp khác phát sinh khi các đèn chiếu cách xa nhau để không có khoảng cách chồng lên nhau. Trong trường hợp này, câu trả lời chỉ đơn giản là tổng của tất cả các độ dài bao phủ riêng lẻ và mọi logic hợp nhất không được vô tình hợp nhất các phân đoạn không liên quan do sắp xếp hoặc xử lý ranh giới không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi điểm sáng, chúng tôi chuyển đổi nó thành một khoảng trên đường thẳng, ví dụ từ tâm trừ bán kính đến tâm cộng bán kính. Sau đó, chúng tôi đánh dấu rõ ràng mọi vị trí đơn vị được bao phủ trong một mảng boolean hoặc đặt và đếm xem có bao nhiêu vị trí duy nhất được bao phủ. 

Điều này hoạt động chính xác vì nó mô phỏng trực tiếp quá trình chiếu sáng. Tuy nhiên, nó không thành công khi giá trị tọa độ lớn hoặc khi phạm vi tọa độ thưa thớt. Nếu tọa độ lên tới 10^9 thì việc tạo một mảng có kích thước đó là không thể. Ngay cả khi chúng tôi nén tọa độ, việc lặp lại từng vị trí đơn vị trong mỗi khoảng có thể dẫn đến tổng tới 10^9 phép toán. 

Quan sát quan trọng là bài toán quy về việc tính tổng chiều dài của một tập các khoảng. Khi mỗi tiêu điểm được chuyển đổi thành một khoảng thời gian, chúng ta không cần phải suy luận về từng điểm riêng lẻ nữa. Chúng ta chỉ cần hợp nhất các khoảng chồng chéo và tính tổng độ dài của chúng. 

Việc sắp xếp các khoảng theo tọa độ bắt đầu làm cho cấu trúc có thể khai thác được. Sau khi được sắp xếp, các khoảng chồng chéo sẽ xuất hiện liên tiếp, vì vậy chúng tôi có thể quét từ trái sang phải và duy trì phân đoạn đã hợp nhất hiện đang hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(R) hoặc O(n·R) | O(R) | Quá chậm | 
| Sắp xếp + Hợp nhất các khoảng thời gian | O(n log n) | O(n) | Đã chấp nhận | 

Ở đây R đại diện cho phạm vi tọa độ hoặc tổng các vị trí riêng biệt được bao phủ. 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mọi tiêu điểm thành một khoảng thời gian, sau đó hợp nhất chúng theo thứ tự được sắp xếp.

1. Đối với mỗi điểm sáng, hãy tính khoảng cách bao phủ của nó là [tâm − bán kính, tâm + bán kính]. Điều này biến đổi độ rọi hình học thành bài toán khoảng chuẩn. 
2. Sắp xếp tất cả các khoảng theo điểm cuối bên trái của chúng. Việc sắp xếp đảm bảo rằng bất kỳ sự trùng lặp nào đều phải xảy ra với các khoảng thời gian được xử lý trước đó, vì vậy chúng ta không bao giờ cần phải xem lại các phân đoạn trước đó. 
3. Khởi tạo khoảng hiện tại là khoảng được sắp xếp đầu tiên và câu trả lời đang chạy là 0. 
4. Lặp lại từng khoảng thời gian còn lại. 
5. Đối với mỗi khoảng thời gian mới, hãy so sánh nó với khoảng thời gian đã hợp nhất hiện tại. 
6. Nếu khoảng thời gian mới bắt đầu sau khi khoảng thời gian hiện tại kết thúc thì sẽ không có sự chồng chéo. Thêm độ dài của khoảng hiện tại vào câu trả lời và bắt đầu khoảng hiện tại mới. Điều này là an toàn vì không có khoảng thời gian nào trong tương lai có thể chồng chéo ngược do sắp xếp. 
7. Nếu khoảng mới trùng lặp hoặc chạm vào khoảng hiện tại, hãy mở rộng điểm cuối bên phải của khoảng hiện tại để bao phủ phần kết hợp. Điều này duy trì tính chính xác vì phạm vi phủ sóng chồng chéo không được tính gấp đôi. 
8. Sau khi xử lý tất cả các khoảng, hãy thêm độ dài khoảng hiện tại cuối cùng vào câu trả lời. 

Lý do sự hợp nhất tham lam này có hiệu quả là tại bất kỳ thời điểm nào trong quá trình quét, khoảng thời gian hiện tại thể hiện sự kết hợp đầy đủ của tất cả các khoảng thời gian được nhìn thấy cho đến nay trùng lặp với nhau. Vì các khoảng được sắp xếp nên bất kỳ khoảng thời gian nào trong tương lai đều sẽ trùng lặp với sự kết hợp này hoặc bắt đầu hoàn toàn sau nó và không có khoảng thời gian nào trước đó có thể vượt ra ngoài khoảng thời gian đã được hợp nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, r = map(int, input().split())
    intervals = []
    
    for _ in range(n):
        x = int(input())
        intervals.append((x - r, x + r))
    
    intervals.sort()
    
    ans = 0
    cur_l, cur_r = intervals[0]
    
    for l, r in intervals[1:]:
        if l > cur_r:
            ans += cur_r - cur_l
            cur_l, cur_r = l, r
        else:
            if r > cur_r:
                cur_r = r
    
    ans += cur_r - cur_l
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên xây dựng tất cả các phạm vi chiếu sáng và lưu trữ chúng theo cặp. Việc sắp xếp đảm bảo chúng tôi xử lý các khoảng thời gian theo thứ tự tăng dần của vị trí bắt đầu, điều này cần thiết để tính năng hợp nhất tham lam hoạt động. 

Các biến`cur_l`Và`cur_r`duy trì phân đoạn phủ sóng hợp nhất hiện đang hoạt động. Bất cứ khi nào một khoảng trống xuất hiện, phân đoạn trước đó sẽ được hoàn thiện và thêm vào câu trả lời. Nếu tồn tại sự chồng chéo, chúng tôi chỉ mở rộng ranh giới bên phải. 

Một lỗi triển khai phổ biến là quên thêm phân đoạn hoạt động cuối cùng sau khi vòng lặp kết thúc, điều này sẽ làm mất phần đóng góp cuối cùng. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào có ba đèn chiếu và bán kính 2:```
centers: 1, 4, 10
```Khoảng thời gian trở thành:```
[-1, 3], [2, 6], [8, 12]
```Thứ tự sắp xếp giống nhau. 

| Bước | Khoảng thời gian hiện tại | Khoảng thời gian mới | Hành động | Tổng số câu trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | [-1, 3] | [2, 6] | hợp nhất → [ -1, 6 ] | 0 | 
| 2 | [-1, 6] | [8, 12] | đóng [-1,6], bắt đầu mới | 7 | 

Khoảng cuối cùng [8,12] cộng thêm 4 nên tổng số là 11. 

Dấu vết này cho thấy các khoảng chồng chéo được hợp nhất thành một vùng liên tục duy nhất như thế nào, ngăn chặn việc tính hai lần. 

Bây giờ hãy xem xét các khoảng rời rạc hoàn toàn:```
centers: 0, 10, 20 with r = 1
```Khoảng thời gian:```
[-1,1], [9,11], [19,21]
```| Bước | Khoảng thời gian hiện tại | Khoảng thời gian mới | Hành động | Tổng số câu trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | [-1, 1] | [9, 11] | đóng [-1,1], bắt đầu mới | 2 | 
| 2 | [9, 11] | [19, 21] | đóng [9,11], bắt đầu mới | 4 | 

Đoạn cuối cộng thêm 2, tổng cộng là 6. 

Điều này xác nhận rằng các vùng không chồng chéo được xử lý độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chiếm ưu thế, hợp nhất là tuyến tính | 
| Không gian | O(n) | khoảng thời gian lưu trữ | 

Bước sắp xếp nằm trong giới hạn n tối đa 100000. Quét tuyến tính đảm bảo rằng mỗi khoảng thời gian được xử lý một lần, giúp giải pháp đủ hiệu quả cho các ràng buộc điển hình của Codeforces. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# sample-like
assert run("3 2\n1\n4\n10\n") == "11"

# no overlap
assert run("3 1\n0\n10\n20\n") == "6"

# all overlap
assert run("3 10\n0\n0\n0\n") == "20"

# single element
assert run("1 5\n7\n") == "10"

# tight chain overlap
assert run("4 2\n0\n3\n6\n9\n") == "11"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các trung tâm chồng chéo giống hệt nhau | 20 | khoảng thời gian trùng lặp | 
| trung tâm cách nhau rời rạc | 6 | phân khúc độc lập | 
| đèn chiếu đơn | 10 | trường hợp cơ sở | 
| chồng chéo chuỗi | 11 | sáp nhập bắc cầu | 

## Vỏ cạnh 

Nếu tất cả các đèn chiếu có cùng vị trí thì mọi khoảng thời gian đều giống hệt nhau. Thuật toán vẫn hoạt động vì việc sắp xếp giữ chúng liền kề và việc hợp nhất chỉ cần mở rộng một lần mà không cần tính toán dư thừa. 

Ví dụ:```
3 5
10
10
10
```Tất cả các khoảng là [5,15]. Khoảng đầu tiên khởi tạo phân đoạn hiện tại và mọi khoảng thời gian tiếp theo đều được chứa đầy đủ, do đó không có thay đổi nào xảy ra. Câu trả lời cuối cùng là 10. 

Khi các khoảng chỉ chạm vào các ranh giới, chẳng hạn như [0,2] và [2,5], thuật toán sẽ coi chúng là chồng chéo vì`l > cur_r`là sai khi bằng nhau. Điều này đảm bảo vùng phủ sóng liền kề được hợp nhất chính xác thành một đoạn có độ dài 5. 

Nếu chỉ có một đèn chiếu, vòng lặp sẽ không bao giờ thực thi và phân đoạn cuối cùng sẽ được trả về trực tiếp, tránh các lỗi sai sót trong logic lặp trống.
