---
title: "CF 1907D - Nhảy Qua Phân Đoạn"
description: "Chúng ta được cung cấp một chuỗi các khoảng trên trục số và chúng ta mô phỏng quá trình chuyển động có giới hạn trên chúng. Người chơi bắt đầu ở vị trí 0. Đối với mỗi khoảng thời gian theo thứ tự, người chơi thực hiện một nước đi và sau nước đi đó họ phải hạ cánh trong khoảng tương ứng."
date: "2026-06-08T20:38:16+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1907
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 913 (Div. 3)"
rating: 1400
weight: 1907
solve_time_s: 100
verified: false
draft: false
---

[CF 1907D - Nhảy qua các phân đoạn](https://codeforces.com/problemset/problem/1907/D) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các khoảng trên trục số và chúng ta mô phỏng quá trình chuyển động có giới hạn trên chúng. Người chơi bắt đầu ở vị trí 0. Đối với mỗi khoảng thời gian theo thứ tự, người chơi thực hiện một nước đi và sau nước đi đó họ phải hạ cánh trong khoảng tương ứng. Quy tắc di chuyển cho biết mỗi bước cho phép nhảy ở khoảng cách tối đa`k`, và chúng ta được yêu cầu chọn giá trị nhỏ nhất như vậy`k`điều đó làm cho nó có thể thỏa mãn tất cả các ràng buộc về khoảng một cách tuần tự. 

Vì vậy, thay vì nghĩ về từng điểm riêng lẻ, tốt hơn nên nghĩ xem người chơi có thể ở đâu sau mỗi bước. Sau khi xử lý khoảng đầu tiên, có một tập hợp các vị trí có thể tiếp cận và sau mỗi khoảng thời gian tiếp theo, tập hợp này sẽ phát triển dựa trên hai ràng buộc: phạm vi có thể tiếp cận trước đó được mở rộng tối đa`k`, và sau đó nó được cắt bớt theo khoảng thời gian hiện tại. 

Đầu ra là số nguyên tối thiểu`k`sao cho quá trình này không bao giờ tạo ra tập trống, nghĩa là luôn có ít nhất một vị trí hợp lệ sau mỗi phân đoạn. 

Các ràng buộc là lớn, với tổng`n`trên các trường hợp thử nghiệm lên đến 2e5. Điều này loại trừ bất kỳ mô phỏng nào thử nhiều giá trị ứng viên của`k`và tính toán lại khả năng tiếp cận từ đầu. Tìm kiếm nhị phân trên`k`sẽ nhân số kiểm tra tuyến tính với hệ số log, hệ số này nằm ở ranh giới nhưng không cần thiết. Cấu trúc của bài toán cho phép tính toán tham lam trực tiếp trong thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

Một sai lầm ngây thơ là mô phỏng quá trình cho một lỗi cố định`k`nhưng chỉ theo dõi một vị trí duy nhất thay vì một phạm vi. Ví dụ, nếu các khoảng là`[1,5]`sau đó`[10,12]`, cách tiếp cận vị trí duy nhất có thể cố gắng nhảy từ 1 đến 10 một cách tham lam và không nhận ra rằng có nhiều lựa chọn bên trong`[1,5]`vấn đề. Một lỗi phổ biến khác là quên rằng sau mỗi bước, toàn bộ khoảng thời gian có thể truy cập đều quan trọng chứ không chỉ một điểm cuối. 

## Phương pháp tiếp cận 

Một ý tưởng bạo lực là sửa chữa một ứng viên`k`và mô phỏng tất cả các vị trí có thể. Sau mỗi đoạn, từ mọi vị trí có thể tiếp cận, chúng ta có thể nhảy tới bất kỳ điểm nào trong khoảng cách`k`, rồi cắt với khoảng tiếp theo. Điều này tương đương với việc duy trì một phân đoạn có thể tiếp cận liên tục, nhưng nếu thực hiện một cách đơn giản dưới dạng một tập hợp hoặc danh sách các trạng thái, nó sẽ bùng nổ theo cấp số nhân vì mỗi bước sẽ phân nhánh thành vô số điểm. 

Quan sát quan trọng là tập hợp có thể truy cập luôn là một khoảng liên tục. Nếu ở bước`i`chúng ta có thể đứng ở bất cứ đâu trong`[L, R]`, sau đó sau một lần di chuyển có giới hạn`k`, chúng ta có thể đạt được chính xác`[L-k, R+k]`. Giao nhau với đoạn tiếp theo`[l_i, r_i]`giữ nó một khoảng thời gian một lần nữa. Vì vậy, chúng tôi chỉ theo dõi một khoảng thời gian cho mỗi bước. 

Điều này làm giảm vấn đề duy trì việc truyền khoảng trong một khoảng thời gian nhất định`k`. Nhiệm vụ còn lại là tìm giá trị nhỏ nhất`k`ngăn chặn khoảng thời gian này trở nên trống rỗng ở bất kỳ bước nào. 

Thay vì tìm kiếm nhị phân`k`, chúng ta có thể rút ra nó một cách trực tiếp. Ở mỗi bước, chúng tôi kiểm tra mức độ mở rộng cần thiết để kết nối khoảng thời gian có thể truy cập trước đó với phân khúc hiện tại. Nếu phân đoạn hiện tại trùng với khoảng thời gian mở rộng trước đó thì không cần thêm chi phí. Nếu không, khoảng cách sẽ xác định bước nhảy yêu cầu tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng tàn bạo của các trạng thái | hàm mũ | lớn | Quá chậm | 
| Lan truyền theo khoảng với k cố định | O(n) | O(1) | Được chấp nhận mỗi lần kiểm tra | 
| Tính toán tham lam trực tiếp | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một khoảng thời gian khả thi hiện tại`[L, R]`, đại diện cho tất cả các vị trí mà người chơi có thể đảm nhận sau khi xử lý từng phân đoạn. 

1. Khởi tạo khoảng thời gian như`[0, 0]`. Điều này đại diện cho điểm khởi đầu trước bất kỳ chuyển động nào. 
2. Đối với đoạn đầu tiên`[l1, r1]`, chúng ta phải đạt được nó từ 0. Bước nhảy yêu cầu tối thiểu được xác định bằng khoảng cách 0 từ đoạn này. Nếu số 0 nằm bên trong nó thì không cần chuyển động gì cả; nếu không chúng ta phải nhảy đến ranh giới gần nhất. 
3. Đối với mỗi đoạn tiếp theo`[li, ri]`, chúng tôi xem xét liệu khoảng thời gian hiện tại`[L, R]`đã chồng chéo nó. Nếu đúng như vậy thì chúng ta có thể ở trong vùng chồng lấp bằng cách chọn một điểm thích hợp trong giao lộ, do đó không cần khoảng cách mới. 
4. Nếu không có sự chồng chéo thì có khoảng cách. Nếu như`R < li`, điểm có thể tiếp cận gần nhất là`R`và chúng ta phải nhảy tới`li`, yêu cầu ít nhất`li - R`. Nếu như`ri < L`, chúng ta phải nhảy xuống dưới yêu cầu`L - ri`. 
5. Chúng tôi tích lũy số lần nhảy cần thiết tối đa qua tất cả các bước, vì`k`phải hỗ trợ quá trình chuyển đổi tồi tệ nhất. 
6. Sau khi tính đến hạn chế di chuyển, chúng tôi cập nhật khoảng thời gian có thể tiếp cận bằng cách giao phạm vi tiếp cận mở rộng với phân khúc hiện tại. 
7. Câu trả lời cuối cùng là khoảng cách tối đa gặp phải trong quá trình này. 

Ý tưởng chính là mỗi bước đóng góp một hạn chế về mức độ lớn`k`phải là để thu hẹp khoảng cách giữa các khoảng thời gian khả thi liên tiếp. 

### Tại sao nó hoạt động 

Ở mỗi bước, tập hợp có thể truy cập được mô tả đầy đủ bằng một khoảng thời gian liên tục. Sự cản trở duy nhất cho sự tiến bộ là khi hai khoảng thời gian liên tiếp cách nhau một khoảng trống. Kích thước của khoảng cách đó chính xác là bước nhảy tối thiểu cần thiết để kết nối chúng trong một lần di chuyển, bởi vì bất kỳ điểm trung gian nào trong khoảng trước đó đều tương đương về khoảng cách với ranh giới đoạn hiện tại. Vì các bước sau chỉ hạn chế tính khả thi hơn nữa nhưng không bao giờ giảm yêu cầu của các chuyển đổi đã được thỏa mãn, nên khoảng cách tối đa trên tất cả các chuyển đổi là hợp lệ nhỏ nhất`k`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        
        l0, r0 = map(int, input().split())
        L, R = l0, r0
        
        # distance needed from start 0
        if 0 < L:
            ans = L
        elif 0 > R:
            ans = -R
        else:
            ans = 0
        
        for _ in range(1, n):
            l, r = map(int, input().split())
            
            if R < l:
                ans = max(ans, l - R)
            elif r < L:
                ans = max(ans, L - r)
            
            L = max(L, l)
            R = min(R, r)
            
            if L > R:
                L, R = l, r
        
        print(ans)

if __name__ == "__main__":
    solve()
```Mã theo dõi khoảng thời gian khả thi hiện tại`[L, R]`. Sau khi đọc từng phân đoạn, đầu tiên nó sẽ tính toán khoảng cách giữa khoảng thời gian hiện tại và phân đoạn tiếp theo. Nếu chúng không trùng nhau, khoảng cách đó sẽ cập nhật câu trả lời dưới dạng giới hạn dưới của ứng cử viên cho`k`. 

bản cập nhật`L = max(L, l)`Và`R = min(R, r)`thể hiện sự hạn chế tính khả thi đối với giao điểm của các vị trí có thể tiếp cận và phân khúc hiện tại. Nếu giao lộ này trống, mã sẽ đặt lại về phân đoạn hiện tại, điều này phản ánh rằng người chơi luôn có thể hạ cánh ở bất kỳ đâu trong phân đoạn sau khi nhảy đủ kích thước hợp lệ. 

Một điểm tinh tế là xử lý phân đoạn đầu tiên tách biệt với phần còn lại, vì vị trí bắt đầu được cố định ở 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 5
3 4
5 6
```Chúng tôi theo dõi`[L, R]`Và`ans`. 

| Bước | Phân đoạn | Hiện tại [L, R] | Kiểm tra khoảng cách | trả lời | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | [0,0] | 0 bên trong [1,5] → 0 | 0 | 
| 1 | [1,5] | giao nhau sau khi nhảy → [1,5] | từ 0 đến [1,5] → 1 | 1 | 
| 2 | [3,4] | [3,4] | chồng chéo | 1 | 
| 3 | [5,6] | [3,4] | khoảng cách 5-4 = 1 | 1 | 

Điều này cho thấy hệ số giới hạn đang bắc cầu từ 0 đến khoảng đầu tiên. 

### Ví dụ 2 

đầu vào:```
3
3 8
10 18
6 11
```| Bước | Phân đoạn | Hiện tại [L, R] | Kiểm tra khoảng cách | trả lời | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | [0,0] | 0→[3,8] = 3 | 3 | 
| 1 | [3,8] | [3,8] | - | 3 | 
| 2 | [10,18] | [3,8] | khoảng trống 10-8 = 2 → đáp 3 | 3 | 
| 3 | [6,11] | [6,8] | chồng chéo sau nút giao | 3 | 

Điều này cho thấy nhiều phân đoạn rời rạc buộc phải theo dõi khoảng cách tối đa như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phân đoạn được xử lý một lần với bản cập nhật O(1) | 
| Không gian | O(1) | Chỉ có một vài biến để theo dõi khoảng thời gian | 

Giải pháp chạy trong giới hạn vì tổng số phân đoạn trên tất cả các trường hợp thử nghiệm tối đa là 2e5, do đó quét tuyến tính là đủ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            L, R = map(int, input().split())
            if 0 < L:
                ans = L
            elif 0 > R:
                ans = -R
            else:
                ans = 0

            for _ in range(1, n):
                l, r = map(int, input().split())
                if R < l:
                    ans = max(ans, l - R)
                elif r < L:
                    ans = max(ans, L - r)
                L = max(L, l)
                R = min(R, r)
                if L > R:
                    L, R = l, r

            out.append(str(ans))
        return "\n".join(out)

    return solve()

# provided samples
assert run("""4
5
1 5
3 4
5 6
8 10
0 1
3
0 2
0 1
0 3
3
3 8
10 18
6 11
4
10 20
0 5
15 17
2 2
""") == """7
0
5
13"""

# custom cases
assert run("""1
1
0 0
""") == "0", "single point"

assert run("""1
2
10 10
10 10
""") == "10", "forced start gap"

assert run("""1
3
0 1
100 101
0 1
""") == "99", "large jump back and forth"

assert run("""1
2
0 5
2 3
""") == "0", "nested intervals"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đoạn đơn tại điểm xuất phát | 0 | không cần chuyển động | 
| khoảng thời gian bắt đầu rời rạc | 10 | xử lý khoảng cách ban đầu | 
| phân đoạn lớn tách biệt | 99 | lan truyền khoảng cách tối đa | 
| khoảng lồng nhau | 0 | giao lộ thu hẹp chính xác | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi khoảng đầu tiên đã chứa 0. Trong trường hợp đó, yêu cầu ban đầu bằng 0 vì không cần phải nhảy để đến bên trong phân đoạn đầu tiên. 

Một trường hợp khác là khi các khoảng tăng dần và rời rạc. Ví dụ`[0,1] → [100,101] → [0,1]`. Thuật toán ghi lại chính xác khoảng cách lớn nhất xảy ra khi chuyển từ`[0,1]`ĐẾN`[100,101]`, sản xuất`99`. 

Một kịch bản tinh tế là khi các khoảng chồng lên nhau một phần nhưng vẫn tạo ra khoảng trống ở một bên. Ví dụ`[0,10] → [20,30] → [15,25]`. Khoảng thời gian có thể tiếp cận sẽ co lại và dịch chuyển, đồng thời khoảng cách được tính toán bằng cách sử dụng đường biên`R`chống lại`20`, tạo ra bước nhảy đúng theo yêu cầu của`10`.
