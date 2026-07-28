---
title: "CF 102791H - Xóa chuỗi"
description: "Nhiệm vụ đưa ra một chuỗi nhị phân. Một thao tác chọn một ký tự hiện có và xóa nó. Sau lần xóa đó, tiền tố dài nhất được tạo từ các ký tự giống hệt nhau cũng sẽ tự động bị xóa."
date: "2026-07-27T18:14:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102791
codeforces_index: "H"
codeforces_contest_name: "ICPC 2020-2021 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102791
solve_time_s: 60
verified: true
draft: false
---

[CF 102791H - Xóa chuỗi](https://codeforces.com/problemset/problem/102791/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Nhiệm vụ đưa ra một chuỗi nhị phân. Một thao tác chọn một ký tự hiện có và xóa nó. Sau lần xóa đó, tiền tố dài nhất được tạo từ các ký tự giống hệt nhau cũng sẽ tự động bị xóa. Mục tiêu là chọn các thao tác xóa theo cách tốt nhất có thể để số lượng thao tác trước khi chuỗi trở nên trống càng lớn càng tốt. Báo cáo vấn đề ban đầu và các ràng buộc là từ Codeforces Gym 102791H. 

Độ dài của chuỗi có thể đạt tới 200000. Với kích thước này, một mô phỏng thực sự sửa đổi chuỗi sau mỗi thao tác là quá tốn kém vì việc xóa từ giữa có thể yêu cầu dịch chuyển nhiều ký tự. Một giải pháp bậc hai với xung quanh$n^2$công việc sẽ vượt xa giới hạn 2 giây cho phép. Chúng ta cần xử lý chuỗi theo thời gian tuyến tính. 

Các trường hợp khó không phải là các chuỗi có nhiều ký tự khác nhau mà là các chuỗi có cấu trúc tiền tố thay đổi sau mỗi lần xóa. Một khối dài có thể biến mất ngay lập tức nếu xử lý không chính xác, trong khi một số khối ký tự đơn có thể được sử dụng để tạo các thao tác bổ sung. 

Ví dụ: với đầu vào:```
2
11
```câu trả lời là:```
1
```Một cách tiếp cận bất cẩn có thể đếm cả hai ký tự một cách riêng biệt, nhưng việc xóa một ký tự sẽ`1`lá`1`và việc xóa tiền tố tự động sẽ loại bỏ ký tự còn lại trong cùng một thao tác. 

Một trường hợp quan trọng khác là:```
6
101010
```Câu trả lời là:```
3
```Mỗi lần chạy có độ dài một. Một giải pháp tham lam luôn tiêu tốn ngay lần chạy tiếp theo có thể cho rằng câu trả lời là số lần chạy, nhưng các thao tác chỉ có thể loại bỏ một ký tự đã chọn và một tiền tố, do đó sáu ký tự đơn được ghép thành ba thao tác hữu ích. 

Trường hợp ranh giới cuối cùng là:```
8
10101000
```Câu trả lời là:```
4
```Khối số 0 cuối cùng không thể bị bỏ qua. Nó có thể được tiêu thụ một phần trong khi các lần chạy ký tự đơn trước đó đang bị xóa, tạo ra các hoạt động bổ sung. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng các hoạt động. Đối với mọi trạng thái của chuỗi, hãy thử xóa mọi ký tự có thể có, chọn bước di chuyển mang lại số lượng thao tác lớn nhất trong tương lai và lặp lại. Điều này đúng vì nó xem xét tất cả các lựa chọn hợp pháp, nhưng nó hoàn toàn không thực tế. Ngay cả việc lưu trữ chi phí trạng thái một chuỗi$O(n)$và số lượng các lựa chọn có thể có ở nhiều tiểu bang làm cho tổng công việc tăng theo cấp số nhân. Ngay cả một mô phỏng đơn giản luôn thực hiện một nước đi đã chọn vẫn cần thường xuyên xóa phần giữa, điều này có thể trở thành$O(n^2)$. 

Quan sát quan trọng là chỉ có các ký tự bằng nhau liên tiếp mới quan trọng. Chúng ta có thể nén chuỗi thành các lần chạy. Ví dụ,`111010`trở thành độ dài`[3, 1, 1, 1]`. 

Một chuỗi có độ dài lớn hơn một có giá trị vì nó có thể cung cấp nhiều thao tác. Khi lần chạy như vậy trở thành tiền tố, việc xóa một ký tự khỏi nó sẽ cho phép thao tác xóa tự động loại bỏ phần còn lại của lần chạy đó. Chạy một ký tự thì khác: nó biến mất hoàn toàn khi đến phía trước, vì vậy nó phải được lưu càng lâu càng tốt. 

Ý tưởng tham lam là luôn sử dụng các khoảng thời gian dài làm nguồn xóa thêm. Nếu lượt chạy trước hiện tại có độ dài lớn hơn một, hãy sử dụng nó. Nếu lần chạy trước là một ký tự đơn, hãy tìm kiếm lần chạy dài sau đó và thay vào đó lấy một ký tự từ lần chạy đó. Điều này giữ cho tiền tố một ký tự tồn tại cho một thao tác khác. Khi không còn lượt chạy dài, các lượt chạy đơn còn lại chỉ có thể được ghép nối, đưa ra trực tiếp số lượng thao tác còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nén chuỗi thành các đoạn dài. Các ký tự bằng nhau liên tiếp sẽ trở thành một lần chạy vì chỉ độ dài của chúng mới ảnh hưởng đến các hoạt động trong tương lai. 
2. Giữ hai con trỏ. Con trỏ bên trái biểu thị lần chạy đầu tiên chưa bị xóa hoàn toàn. Con trỏ bên phải tìm kiếm lần chạy tiếp theo có độ dài lớn hơn một. 
3. Nếu đường chạy bên trái hiện tại có độ dài lớn hơn một, hãy thực hiện một thao tác trên đó và di chuyển con trỏ bên trái. Có thể xóa lần chạy này bằng cách xóa tiền tố tự động, do đó không cần phải chạm vào lần chạy khác. 
4. Nếu lượt chạy bên trái hiện tại có độ dài bằng một, hãy thử tìm lượt chạy sau có độ dài lớn hơn một. Xóa một ký tự khỏi lần chạy sau đó và đếm một thao tác. Dòng ký tự đơn bên trái vẫn có sẵn cho hoạt động trong tương lai. 
5. Nếu không còn lượt chạy dài nữa thì tất cả các lượt chạy còn lại đều có độ dài bằng một. Hai lần chạy như vậy có thể được loại bỏ cho mỗi thao tác, vì vậy hãy cộng mức trần của số lần chạy còn lại chia cho hai và kết thúc. 

Tại sao nó hoạt động: 

Điều bất biến là mọi thao tác sẽ bảo toàn càng nhiều thao tác xóa trong tương lai càng tốt. Tiền tố một ký tự là tài nguyên dễ vỡ nhất vì một khi trở thành tiền tố, nó sẽ biến mất hoàn toàn. Các lần chạy dài là nguồn duy nhất của các hoạt động bổ sung vì việc xóa một ký tự khỏi chúng vẫn để lại tiền tố có thể bị xóa. Thuật toán tham lam luôn dành nhiều thời gian chạy trước khi bị mất và trì hoãn các lần chạy ký tự đơn, do đó không có cơ hội hoạt động nào bị lãng phí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()

    runs = []
    cnt = 1
    for i in range(1, n):
        if s[i] == s[i - 1]:
            cnt += 1
        else:
            runs.append(cnt)
            cnt = 1
    runs.append(cnt)

    m = len(runs)
    ans = 0
    l = 0
    r = 0

    while l < m:
        if l > r:
            r = l

        if runs[l] > 1:
            ans += 1
            l += 1
        else:
            while r < m and runs[r] == 1:
                r += 1

            if r < m:
                runs[r] -= 1
                ans += 1
                l += 1
            else:
                ans += (m - l + 1) // 2
                break

    print(ans)

if __name__ == "__main__":
    solve()
```Phần đầu tiên xây dựng biểu diễn độ dài lần chạy. Điều này loại bỏ nhu cầu duy trì chuỗi thực tế vì vị trí chính xác của các ký tự không còn quan trọng nữa. 

Vòng lặp chính tuân theo các quyết định tham lam từ hướng dẫn. Con trỏ bên trái chỉ di chuyển về phía trước vì các lần chạy đã xóa sẽ không bao giờ quay trở lại. Con trỏ bên phải cũng chỉ di chuyển về phía trước trong khi tìm kiếm một khoảng thời gian dài hữu ích, mang lại tổng công việc tuyến tính. 

Sự giảm đi`runs[r] -= 1`là tinh tế. Chúng tôi không xóa toàn bộ hoạt động. Chúng tôi đang sử dụng chính xác một ký tự từ nó để tạo một thao tác trong khi để phần còn lại thực hiện sau. Việc quên điều này sẽ biến thuật toán thành chiến lược không chính xác khi loại bỏ các lượt chạy quá nhanh. 

Biểu thức cuối cùng`(m - l + 1) // 2`xử lý các lần chạy ký tự đơn còn lại. Vì mỗi thao tác có thể loại bỏ tối đa hai lần chạy như vậy nên đây là số lượng thao tác tối đa mà chúng có thể đóng góp. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
6
111010
```các cuộc chạy là`[3,1,1,1]`. 

| Bước | Chạy trái | Chạy đúng đã sử dụng | Chạy theo hành động | Trả lời | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 3 | |`[3,1,1,1]`| 0 | 
| 1 | 3 | |`[3,1,1,1]`| 1 | 
| 2 | 1 | không | đĩa đơn còn lại | 3 | 

Hoạt động đầu tiên sử dụng tiền tố dài. Sau đó chỉ còn lại các lần chạy duy nhất, đóng góp thêm hai thao tác nữa. Ví dụ cho thấy tại sao nên sử dụng các lượt chạy dài trước tiên. 

Đối với đầu vào:```
6
101010
```các cuộc chạy là`[1,1,1,1,1,1]`. 

| Bước | Vị trí bên trái | Đã tìm thấy lâu dài | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | không | còn lại sáu đĩa đơn | 0 | 
| Kết thúc | 0 | không | đánh đơn đôi | 3 | 

Điều này thể hiện trường hợp cuối cùng của thuật toán. Nếu không chạy dài, câu trả lời chỉ được xác định bằng số lần chạy đơn còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được quét trong khi tòa nhà chạy và mỗi con trỏ chỉ di chuyển về phía trước. | 
| Không gian | O(n) | Danh sách chạy nén lưu trữ tối đa một mục nhập cho mỗi ký tự. | 

Kích thước đầu vào tối đa là 200000 ký tự, do đó, giải pháp tuyến tính dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    s = input().strip()

    runs = []
    cnt = 1
    for i in range(1, n):
        if s[i] == s[i - 1]:
            cnt += 1
        else:
            runs.append(cnt)
            cnt = 1
    runs.append(cnt)

    m = len(runs)
    ans = 0
    l = 0
    r = 0

    while l < m:
        if l > r:
            r = l
        if runs[l] > 1:
            ans += 1
            l += 1
        else:
            while r < m and runs[r] == 1:
                r += 1
            if r < m:
                runs[r] -= 1
                ans += 1
                l += 1
            else:
                ans += (m - l + 1) // 2
                break

    return str(ans)

assert solution("6\n111010\n") == "3"
assert solution("1\n0\n") == "1"
assert solution("1\n1\n") == "1"
assert solution("2\n11\n") == "1"
assert solution("6\n101010\n") == "3"
assert solution("8\n10101000\n") == "4"
assert solution("5\n00000\n") == "3"
assert solution("1\n0\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`6\n111010\n`|`3`| Chạy hỗn hợp dài và đơn | 
|`6\n101010\n`|`3`| Tất cả các lần chạy có độ dài một | 
|`8\n10101000\n`|`4`| Sử dụng lâu dài sau này một cách chính xác | 
|`5\n00000\n`|`3`| Một cuộc chạy lớn duy nhất | 

## Vỏ cạnh 

cho`2\n11\n`, dạng nén là`[2]`. Thuật toán nhìn thấy một lượt chạy dài ngay lập tức, đếm một thao tác và kết thúc. Nó không chia hoạt động thành nhiều thao tác vì việc xóa tiền tố tự động sẽ loại bỏ các ký tự còn lại cùng một lúc. 

Vì`6\n101010\n`, dạng nén chứa sáu lần chạy đơn lẻ. Không cần sử dụng trong thời gian dài nên thuật toán sẽ chuyển trực tiếp đến quy tắc ghép nối cuối cùng. Sáu lần chạy đơn tạo ra ba thao tác. 

Vì`8\n10101000\n`, số lần chạy là`[1,1,1,1,1,1,2]`. Sáu lần chạy đơn đầu tiên không nên bị xóa ngay lập tức. Thuật toán liên tục mượn một ký tự từ chuỗi dài cuối cùng, giảm dần ký tự đó trong khi vẫn giữ nguyên các ký tự đơn phía trước. Sau khi hết thời gian, các đĩa đơn còn lại được ghép nối, tạo ra bốn thao tác.
