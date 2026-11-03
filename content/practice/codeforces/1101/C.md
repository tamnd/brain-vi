---
title: "CF 1101C - Phòng và Công đoàn"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm cung cấp một tập hợp các phân đoạn đóng trên một trục số. Nhiệm vụ là gán mọi phân đoạn cho một trong hai nhóm sao cho không có phân đoạn nào trong nhóm này trùng lặp với bất kỳ phân đoạn nào trong nhóm kia."
date: "2026-06-13T07:21:10+07:00"
tags: ["codeforces", "competitive-programming", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1101
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 58 (Rated for Div. 2)"
rating: 1500
weight: 1101
solve_time_s: 417
verified: false
draft: false
---

[CF 1101C - Phân khu và Liên minh](https://codeforces.com/problemset/problem/1101/C) 

**Đánh giá:** 1500 
**Thẻ:** sắp xếp 
**Thời gian giải:** 6 phút 57 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm cung cấp một tập hợp các phân đoạn đóng trên một trục số. Nhiệm vụ là gán mọi phân đoạn cho một trong hai nhóm sao cho không có phân đoạn nào trong nhóm này trùng lặp với bất kỳ phân đoạn nào trong nhóm kia. Sự chồng chéo ở đây có nghĩa là giao điểm của hai đoạn không trống, bao gồm cả việc chạm vào một điểm duy nhất. 

Một cách khác để xem yêu cầu là nếu chúng ta chọn bất kỳ đoạn nào từ nhóm 1 và bất kỳ đoạn nào từ nhóm 2 thì hai đoạn đó phải tách rời hoàn toàn trên đường thẳng. 

Chúng ta phải quyết định xem liệu sự phân chia các phân đoạn như vậy có tồn tại hay không và nếu có thì đưa ra một phép gán hợp lệ. 

Các ràng buộc đủ chặt chẽ để bất kỳ giải pháp nào so sánh tất cả các cặp phân đoạn trong mỗi trường hợp thử nghiệm đều sẽ thất bại. Trong trường hợp xấu nhất, một thử nghiệm duy nhất chứa 100000 phân đoạn và có tới 50000 trường hợp thử nghiệm. Kiểm tra bậc hai trong mỗi bài kiểm tra sẽ bao hàm tổng cộng khoảng 10^10 so sánh, vượt xa giới hạn 2 giây. Điều này ngay lập tức loại trừ việc kiểm tra theo cặp hoặc bất kỳ phương pháp nào liên tục quét toàn bộ tập hợp cho từng phân đoạn. 

Trường hợp góc tinh tế xuất hiện khi tất cả các phân đoạn được kết nối thông qua các phần chồng chéo, thậm chí là gián tiếp. Ví dụ: nếu đoạn A chồng lên B và B chồng lên C, ngay cả khi A không chồng lên C, cả ba đều tạo thành một cấu trúc được kết nối một cách hiệu quả. Trong trường hợp đó, việc tách chúng thành hai nhóm không có giao điểm chéo sẽ không thể thực hiện được nếu cả hai nhóm đều không trống. Tình huống này tương ứng với toàn bộ tập hợp tạo thành một thành phần được kết nối duy nhất dưới sự chồng chéo. 

Một trường hợp khác xuất hiện khi các phân đoạn đã được phân tách rõ ràng bằng một “khoảng trống”. Ví dụ: nếu tất cả các phân đoạn nằm trong [1, 10] hoặc [20, 30] không có sự trùng lặp giữa hai cụm thì một phân vùng hợp lệ sẽ tồn tại một cách tự nhiên. Thách thức ở đây là phát hiện xem sự phân chia như vậy có tồn tại hay không và xây dựng nó một cách hiệu quả. 

## Phương pháp tiếp cận 

Ý tưởng đơn giản là coi mỗi đoạn như một nút trong biểu đồ và kết nối hai nút nếu các đoạn của chúng giao nhau. Khi đó điều kiện trở thành: chúng ta phải gán hai màu cho các nút sao cho không có cạnh nào kết nối các màu khác nhau. Tuy nhiên, đây không phải là điều kiện lưỡng cực tiêu chuẩn, vì chúng ta không bắt buộc phải tránh các cạnh cùng màu, chúng ta buộc phải tránh các cạnh có màu chéo. Điều đó đảo ngược logic: thay vì tách các nút liền kề, chúng ta phải đảm bảo rằng mọi thành phần được kết nối hoàn toàn nằm trong một nhóm. 

Vì vậy, giải pháp đơn giản trước tiên sẽ xây dựng biểu đồ giao lộ đầy đủ bằng cách kiểm tra từng cặp đoạn thẳng. Nếu hai phân đoạn trùng nhau, chúng ta sẽ hợp nhất chúng thành một thành phần. Sau đó, chúng tôi cố gắng gán các thành phần cho hai nhóm. Nhưng nếu chỉ có một thành phần thì không thể chia nó thành hai nhóm không rỗng nên đáp án là không thể. Nếu có ít nhất hai thành phần, chúng ta có thể gán toàn bộ từng thành phần vào nhóm 1 hoặc nhóm 2, đảm bảo cả hai nhóm đều không trống. 

Vấn đề là xây dựng kết cấu nút giao. Kiểm tra tất cả các cặp tốn O(n^2). Ngay cả khi chúng tôi tối ưu hóa một chút, nó vẫn quá chậm. 

Quan sát quan trọng là khả năng kết nối giao lộ trên các đoạn được nắm bắt bằng cách sắp xếp. Nếu chúng ta sắp xếp các phân đoạn theo điểm cuối bên trái và quét, chúng ta có thể hợp nhất bất kỳ chuỗi chồng chéo nào thành một thành phần khoảng duy nhất. Bất cứ khi nào một phân đoạn bắt đầu sau điểm cuối bên phải tối đa hiện tại, chúng tôi biết một thành phần mới sẽ bắt đầu. 

Như vậy, thay vì xây dựng biểu đồ, chúng ta chỉ cần hợp nhất các khoảng thành các thành phần được kết nối bằng một lượt duy nhất sau khi sắp xếp. Khi các thành phần được xác định, vấn đề sẽ giảm xuống còn việc kiểm tra xem có nhiều hơn một thành phần hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (biểu đồ chồng chéo theo cặp) | O(n^2) | O(n^2) | Quá chậm | 
| Tối ưu (sắp xếp + gộp quét) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc tất cả các phân đoạn và giữ lại chỉ số ban đầu của chúng. Chúng ta cần chỉ số vì đầu ra phải khớp với thứ tự đầu vào chứ không phải thứ tự sắp xếp. 
2. Sắp xếp các đoạn theo điểm cuối bên trái của chúng. Việc sắp xếp là cần thiết vì nó cho phép chúng tôi phát hiện sự trùng lặp bằng cách sử dụng một lần quét thay vì so sánh theo cặp. 
3. Khởi tạo khoảng thành phần hiện tại bằng cách sử dụng phân đoạn đầu tiên. Đồng thời duy trì bộ đếm id thành phần. 
4. Đi qua các đoạn từ trái sang phải. Đối với mỗi phân đoạn, hãy so sánh điểm cuối bên trái của nó với điểm cuối bên phải tối đa của thành phần hiện tại. 
5. Nếu phân đoạn hiện tại bắt đầu trước hoặc ở điểm cuối bên phải tối đa hiện tại, thì phân đoạn đó sẽ trùng lặp với thành phần hiện tại, vì vậy chúng tôi hợp nhất nó bằng cách mở rộng điểm cuối bên phải tối đa hiện tại nếu cần. 
6. Nếu nó bắt đầu sau điểm cuối bên phải tối đa hiện tại, điều này có nghĩa là nó không giao nhau với bất kỳ thứ gì trong thành phần hiện tại. Chúng tôi hoàn thiện thành phần trước đó và bắt đầu một thành phần mới. 
7. Sau khi tất cả các phân đoạn được xử lý, chúng ta có một phân vùng gồm các phân đoạn thành các thành phần được kết nối trong đó mỗi thành phần là một chuỗi tối đa các phân đoạn chồng chéo. 
8. Nếu chỉ có một thành phần, ghi -1 vì bất kỳ sự phân chia nào cũng sẽ buộc ít nhất một thành phần trùng lặp giữa các nhóm. 
9. Mặt khác, gán nhóm 1 cho tất cả các phân đoạn trong thành phần đầu tiên và nhóm 2 cho tất cả các thành phần còn lại. 

### Tại sao nó hoạt động 

Hai đoạn nằm trong cùng một thành phần khi tồn tại một chuỗi các đoạn giao nhau theo cặp kết nối chúng. Bất kỳ phân khúc nào trong một thành phần sẽ chồng chéo (trực tiếp hoặc gián tiếp) với mọi phân khúc khác trong thành phần đó, do đó, việc đặt chúng vào các nhóm khác nhau sẽ tạo ra sự giao nhau giữa các nhóm. Ngược lại, các phân đoạn trong các thành phần khác nhau được phân tách bằng một khoảng trống trong quá trình quét được sắp xếp, nghĩa là không phân đoạn nào trong thành phần này có thể chồng lên bất kỳ phân đoạn nào trong thành phần khác. Điều này đảm bảo rằng việc gán toàn bộ thành phần cho các nhóm sẽ tạo ra một giải pháp hợp lệ và giải pháp đó tồn tại khi và chỉ khi có ít nhất hai thành phần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        segs = []
        for i in range(n):
            l, r = map(int, input().split())
            segs.append((l, r, i))

        segs.sort()

        comp = [0] * n
        cid = 0

        cur_l, cur_r, idx = segs[0]

        comp[idx] = cid

        for l, r, i in segs[1:]:
            if l <= cur_r:
                if r > cur_r:
                    cur_r = r
                comp[i] = cid
            else:
                cid += 1
                cur_r = r
                comp[i] = cid

        if cid == 0:
            out.append("-1")
            continue

        ans = [0] * n
        for i in range(n):
            ans[i] = 1 if comp[i] == 0 else 2

        out.append(" ".join(map(str, ans)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc sắp xếp theo điểm cuối bên trái để việc phát hiện chồng chéo giảm xuống mức so sánh đơn giản với điểm cuối bên phải tối đa đang chạy. Biến`cur_r`là trạng thái chính: nó theo dõi phạm vi tiếp cận xa nhất của thành phần được kết nối hiện tại. Khi một phân khúc nằm trong phạm vi này, nó sẽ được hấp thụ vào cùng một thành phần. Khi nó nằm bên ngoài, chúng ta bắt đầu một thành phần mới. 

Một cạm bẫy phổ biến là quên bảo toàn các chỉ số gốc. Vì việc sắp xếp sắp xếp lại các phân đoạn nên chúng ta phải lưu trữ các bài tập trở lại thứ tự ban đầu. Một vấn đề tế nhị khác là việc hợp nhất chỉ yêu cầu theo dõi điểm cuối bên phải tối đa; việc theo dõi ranh giới bên trái là không cần thiết vì sự trùng lặp được xác định hoàn toàn bằng việc liệu phân đoạn tiếp theo có bắt đầu trước khi phạm vi tiếp cận hiện tại kết thúc hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2
5 5
2 3
```Các phân đoạn được sắp xếp trở thành: 

(2,3, idx1), (5,5, idx0) 

| Bước | Phân đoạn | cur_r | Thành phần | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | (2,3) | 3 | 0 | bắt đầu thành phần 0 | 
| 2 | (5,5) | 3 → 5 | 1 | thành phần mới | 

Phân công đầu ra trở thành: 

thành phần 0 → nhóm 1, thành phần 1 → nhóm 2 

Vì vậy đầu ra là`2 1`. 

Điều này xác nhận rằng các phân đoạn không chồng chéo được tách biệt rõ ràng thành các thành phần khác nhau. 

### Ví dụ 2 

đầu vào:```
3
3
3 5
2 3
2 3
```Đã sắp xếp: 

(2,3), (2,3), (3,5) 

| Bước | Phân đoạn | cur_r | Thành phần | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | (2,3) | 3 | 0 | bắt đầu | 
| 2 | (2,3) | 3 | 0 | hợp nhất | 
| 3 | (3,5) | 5 | 0 | hợp nhất | 

Chỉ có một thành phần được hình thành. 

Vì tất cả các phân đoạn được kết nối thông qua sự chồng chéo nên bất kỳ sự phân chia nào cũng sẽ buộc một nhóm trống hoặc tạo ra sự chồng chéo giữa các nhóm. Đầu ra là`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, quét là tuyến tính | 
| Không gian | O(n) | lưu trữ các phân đoạn và nhãn thành phần | 

Tổng số phân đoạn trên tất cả các trường hợp thử nghiệm tối đa là 100000, do đó, cách tiếp cận dựa trên sắp xếp phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        segs = []
        for i in range(n):
            l, r = map(int, input().split())
            segs.append((l, r, i))

        segs.sort()

        comp = [0] * n
        cid = 0

        cur_r = segs[0][1]
        comp[segs[0][2]] = 0

        for l, r, i in segs[1:]:
            if l <= cur_r:
                cur_r = max(cur_r, r)
                comp[i] = cid
            else:
                cid += 1
                cur_r = r
                comp[i] = cid

        if cid == 0:
            out.append("-1")
        else:
            ans = [1 if comp[i] == 0 else 2 for i in range(n)]
            out.append(" ".join(map(str, ans)))

    return "\n".join(out)

# provided samples
assert run("""3
2
5 5
2 3
3
3 5
2 3
2 3
3
3 3
4 4
5 5
""") == """2 1
-1
1 1 2"""

# minimum size
assert run("""1
2
1 1
2 2
""") in ["1 2", "2 1"]

# all overlapping
assert run("""1
3
1 5
2 6
3 7
""") == "-1"

# already separated
assert run("""1
3
1 2
10 11
20 21
""").count(" ") == 2
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 đoạn rời rạc | bất kỳ sự phân chia hợp lệ nào | khả năng phân tách cơ bản | 
| chuỗi chồng chéo hoàn toàn | -1 | phát hiện thành phần đơn | 
| ba khối riêng biệt | chia 1/2 hợp lệ | tính đúng đắn của nhiều thành phần | 

## Vỏ cạnh 

Một chuỗi các phân đoạn chồng chéo được kết nối đầy đủ, chẳng hạn như`[1, 5], [2, 6], [3, 7]`, tạo ra một thành phần được hợp nhất trong quá trình quét vì điểm cuối bên phải tối đa đang chạy không bao giờ giảm. Thuật toán gán tất cả các phân đoạn cho thành phần 0 và đưa ra kết quả chính xác`-1`. 

Một bộ hoàn toàn rời rạc như`[1,2], [5,6], [10,11]`tạo ba thành phần vì mỗi phân đoạn mới bắt đầu sau phân đoạn trước đó`cur_r`. Quá trình quét đặt lại chính xác ở mỗi khoảng trống, tạo ra nhiều thành phần và cho phép phân chia hai nhóm hợp lệ. 

Chạm vào các phân đoạn như`[1,3]`Và`[3,5]`được coi là chồng chéo vì`l <= cur_r`. Điều này đảm bảo chúng vẫn ở cùng một thành phần, điều này là bắt buộc vì chúng có chung một điểm và không thể tách rời giữa các nhóm.
