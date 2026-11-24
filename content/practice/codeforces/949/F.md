---
title: "CF 949F - Thiên văn học"
description: "Chúng ta có 2n điểm mạng trong mặt phẳng, mỗi điểm đại diện cho một ngôi sao. Ở đâu đó trong cấu hình thiên văn ban đầu đã tồn tại một điểm đặc biệt, Mặt Trăng, với tọa độ nguyên, sao cho các ngôi sao có thể được phân chia thành n cặp với một hình học rất cứng nhắc…"
date: "2026-06-17T02:24:46+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 949
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 469 (Div. 1)"
rating: 3300
weight: 949
solve_time_s: 69
verified: true
draft: false
---

[CF 949F - Thiên văn học](https://codeforces.com/problemset/problem/949/F) 

**Đánh giá:** 3300 
**Tags:** hình học, xác suất 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có 2n điểm mạng trong mặt phẳng, mỗi điểm đại diện cho một ngôi sao. Ở đâu đó trong cấu hình thiên văn ban đầu tồn tại một điểm đặc biệt, Mặt Trăng, với tọa độ nguyên, sao cho các ngôi sao có thể được chia thành n cặp với một tính chất hình học rất cứng nhắc: với mỗi cặp, đường thẳng xuyên qua hai ngôi sao cũng đi qua Mặt Trăng. Mỗi cặp tạo ra một đường thẳng khác nhau, nhưng tất cả các đường thẳng đó giao nhau ở đúng một điểm chung, đó là Mặt trăng. 

Trong đầu vào hiện đại, chúng ta chỉ thấy tọa độ 2n sao. Sự ghép đôi bị mất và Mặt trăng bị mất tích. Nhiệm vụ là tái tạo lại bất kỳ tọa độ nguyên nào có thể là Mặt trăng hoặc chứng minh rằng không có điểm nào như vậy tồn tại. 

Về mặt hình học, cấu trúc ẩn rất mạnh: tất cả 2n điểm phải được ghép nối sao cho mỗi cặp xác định một đường thẳng đi qua một điểm cố định duy nhất. Điều này tương đương với việc nói rằng mỗi ngôi sao nằm trên một trong n đường thẳng đi qua một tâm chung và mỗi đường thẳng chứa đúng hai ngôi sao. 

Giới hạn tọa độ lớn, có giá trị tuyệt đối lên tới 10^6 và n có thể lớn tới 2600, do đó, bất kỳ phương pháp nào cố gắng kiểm tra tất cả các cặp hoặc tất cả các trung tâm ứng cử viên một cách rõ ràng là không thể. Cấu trúc phải được trích xuất chỉ từ tập hợp điểm. 

Một khó khăn chính là việc ghép nối không xác định được, vì vậy chúng tôi không thể trực tiếp xây dựng lại các dòng. Một vấn đề tinh tế khác là nhiều công trình xây dựng ứng viên hình học có thể tạo ra tọa độ hợp lý cho điểm giao nhau, nhưng đáp án bắt buộc phải tách rời và tách biệt khỏi tất cả các điểm đầu vào. 

Một ý tưởng ngây thơ là chọn hai điểm và cho rằng cấu trúc điểm giữa hoặc giao điểm của chúng xác định Mặt trăng, nhưng điều này nhanh chóng thất bại vì không xác định được cặp điểm chính xác và các cặp điểm khác nhau tạo ra các giao điểm ứng cử viên khác nhau. 

Trường hợp cạnh quan trọng là tính đối xứng. Nếu cấu hình có tính đối xứng cao, nhiều tâm ứng cử viên có thể có giá trị về mặt hình học, nhưng chỉ có một tâm phù hợp với tất cả các cặp. Một chế độ thất bại khác là giả định rằng việc ghép nối hoặc sắp xếp tùy ý theo góc xung quanh một trung tâm được đoán sẽ hoạt động mà không cần xác định trước trung tâm đó. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ cố gắng đoán Mặt trăng và sau đó xác minh xem liệu các điểm có thể được ghép thành các bộ ba thẳng hàng hay không (Mặt trăng với mỗi cặp). Đối với một tâm ứng cử viên cố định M, chúng ta có thể biến đổi mọi điểm thành một vectơ chỉ phương từ M và kiểm tra xem liệu tất cả các vectơ có thể ghép với các hướng giống hệt nhau hay không (các tia ngược chiều). Điều này đòi hỏi phải sắp xếp hoặc băm các vectơ chỉ hướng và xác minh các bội số. 

Vấn đề là không rõ trung tâm ứng cử viên. Không gian của các tọa độ nguyên có thể có là rất lớn, được giới hạn bởi 10^6 theo mỗi hướng, do đó việc liệt kê các ứng cử viên là không thể. Ngay cả việc hạn chế ứng viên ở điểm đầu vào hoặc điểm giữa của các cặp vẫn để lại khả năng O(n^2), là quá lớn. 

Quan sát quan trọng là tâm đúng bị hạn chế rất nhiều bởi tính đối xứng của sự khác biệt. Giả sử tâm đúng là C. Với bất kỳ cặp đúng nào (A, B), chúng ta có C nằm trên đoạn AB mở rộng, nghĩa là các vectơ thỏa mãn C = A + B - C được sắp xếp lại ngụ ý A + B = 2C. Vì vậy, mọi cặp hợp lệ đều có tổng bằng cùng một vectơ 2C. 

Điều này chuyển đổi vấn đề thành: phân vùng các điểm thành các cặp sao cho tổng các cặp không đổi. Hằng số đó là 2C. Vì vậy, nếu chúng ta đoán một cặp (A, B), thì 2C = A + B được cố định và C được xác định. Nhiệm vụ còn lại là xác minh xem tất cả các điểm khác có thể ghép được sao cho tổng bằng A + B hay không. 

Điều này làm giảm đáng kể vấn đề: thay vì tìm kiếm C trực tiếp, chúng ta thử các tổng ứng viên được hình thành bằng cách ghép một điểm đã chọn với mọi điểm khác. Đối với mỗi ứng cử viên C như vậy, chúng tôi kiểm tra xem liệu có tồn tại một cặp hợp lệ đầy đủ theo quy tắc cảm ứng hay không.

Bước xác minh có hiệu quả bằng cách băm các phần bù nhiều tập: đối với mỗi điểm P, đối tác bắt buộc của nó là Q = (2C - P). Nếu tất cả các điểm có thể được so khớp duy nhất và không có điểm nào khớp chính xác thì chúng ta có một giải pháp hợp lệ. 

Sự phức tạp xuất phát từ việc thử O(n) trung tâm ứng viên (cố định một điểm và thử ghép nó với một đại diện khác) và mỗi lần xác thực đều tốn O(n log n) hoặc O(n) khi băm. Điều này có thể chấp nhận được với n lên đến 2600. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Ghép đôi / liệt kê trung tâm Brute Force | O(exp(n)) | O(1) | Không thể | 
| Hãy thử ứng viên từ một mỏ neo cố định | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng thực tế là tâm đúng phải thỏa mãn 2C = A + B đối với một số cặp hợp lệ (A, B). Chúng tôi thử mọi khả năng trong đó A cố định và B thay đổi. 

1. Chọn điểm A làm người tham gia tiềm năng trong cặp đầu tiên. 

Điều này đảm bảo chúng tôi neo các công trình xây dựng ứng viên vào một cấu trúc thực thay vì phỏng đoán tùy ý. 
2. Đối với mọi điểm B khác, hãy tính tâm ứng cử viên C = (A + B) / 2 hoặc tính tương đương với S = A + B là tổng hằng số cần thiết. 

Chúng tôi tránh các vấn đề về dấu phẩy động bằng cách làm việc trực tiếp với tổng số nguyên. 
3. Đối với mỗi tổng ứng cử viên S, hãy thử xác nhận xem liệu tất cả các điểm có thể được ghép nối sao cho P + Q = S giữ nguyên hay không. 

Điều này mã hóa yêu cầu hình học rằng tất cả các đường đều đi qua một tâm duy nhất. 
4. Xây dựng bản đồ tần số của tất cả các điểm. Lặp lại qua các điểm theo thứ tự bất kỳ và với mỗi điểm P vẫn chưa được sử dụng, hãy tính Q = S - P. 
5. Nếu Q không có sẵn trong multiset hoặc đã cạn kiệt, hãy loại bỏ ứng cử viên S này ngay lập tức. 
6. Nếu chúng ta khớp thành công tất cả các điểm, hãy xuất ra tâm tương ứng C = S / 2. 

Lý do chúng ta có thể so sánh tham lam P với S - P một cách an toàn là vì mọi giải pháp hợp lệ đều buộc phải ghép cặp tiến trình hoàn hảo dưới ràng buộc tổng, do đó thứ tự không quan trọng miễn là tính nhất quán được duy trì. 

### Tại sao nó hoạt động 

Tất cả các cấu hình hợp lệ ngụ ý tổng vectơ không đổi trên các cặp: với mọi cặp (A, B), A + B = 2C. Ngược lại, nếu chúng ta tìm thấy một giá trị S sao cho tập hợp nhiều điểm có thể được phân chia thành các cặp có tổng bằng S, thì việc xác định C = S / 2 đảm bảo tất cả các dòng đều đi qua C vì mỗi cặp đối xứng quanh C. Việc tìm kiếm trên S xuất phát từ một mỏ neo A là đủ vì ít nhất một cặp đúng phải bao gồm A và cặp đó tạo ra tổng thực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(2 * n)]
    
    from collections import Counter

    def try_sum(Sx, Sy):
        cnt = Counter(pts)
        for x, y in pts:
            if cnt[(x, y)] == 0:
                continue
            cnt[(x, y)] -= 1
            tx, ty = Sx - x, Sy - y
            if cnt[(tx, ty)] == 0:
                cnt[(x, y)] += 1
                return None
            cnt[(tx, ty)] -= 1
        
        return (Sx // 2, Sy // 2)

    # try all candidates using point 0 paired with others
    for i in range(1, 2 * n):
        sx = pts[0][0] + pts[i][0]
        sy = pts[0][1] + pts[i][1]
        res = try_sum(sx, sy)
        if res is not None:
            cx, cy = res
            # ensure center is not a star
            if (cx, cy) not in pts:
                print("Yes")
                print(cx, cy)
                return

    print("No")

if __name__ == "__main__":
    solve()
```Mã này sửa một điểm tham chiếu và lặp lại tất cả các đối tác có thể có để tạo ra các tổng ứng viên. Đối với mỗi ứng cử viên, nó xây dựng một bộ đếm tần số và khớp một cách tham lam từng điểm không sử dụng với phần bù của nó dưới tổng. Nếu tại bất kỳ thời điểm nào phần bổ sung bị thiếu, ứng cử viên không hợp lệ. 

Kiểm tra cuối cùng đảm bảo rằng trung tâm tính toán không phải là một trong các ngôi sao, theo yêu cầu của báo cáo vấn đề. 

Một chi tiết triển khai tinh tế là chúng tôi luôn giảm số lượng trước khi tìm kiếm phần bù, điều này ngăn cản việc vô tình ghép một điểm với chính nó trừ khi bội số cho phép điều đó. Điều này đảm bảo tính đúng đắn trong các trường hợp đối xứng suy biến. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 1
1 3
3 1
3 3
```Ta thử ghép (1,1) với (3,3), cho S = (4,4), vậy tâm ứng viên là (2,2). 

| Bước | Điểm P | Bắt buộc Q = S - P | Trạng thái truy cập | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | (1,1) | (3,3) | tất cả có mặt | cặp | 
| 2 | (1,3) | (3,1) | tất cả có mặt | cặp | 

Tất cả các điểm đều trùng khớp, vì vậy (2,2) là hợp lệ. 

Điều này xác nhận tính bất biến rằng tất cả các cặp đều đối xứng quanh cùng một điểm giữa. 

### Ví dụ 2 

đầu vào:```
2
0 0
0 2
2 0
2 2
```Thử (0,0) với (2,2) cho S = (2,2), tâm (1,1). 

| Bước | P | Q | Còn lại | 
| --- | --- | --- | --- | 
| (0,0) | (0,0) | (2,2) | hợp lệ | 
| (0,2) | (0,2) | (2,0) | hợp lệ | 

Tất cả các điểm đều khớp, xác nhận tính chính xác. 

Ví dụ này nhấn mạnh rằng nhiều cấu hình đối xứng đều quy về một trung tâm duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Chúng tôi thử các cặp ứng cử viên O(n) và xác thực từng cặp trong O(n) bằng cách sử dụng hàm băm | 
| Không gian | O(n) | Bản đồ tần số lưu trữ tất cả các điểm trong quá trình xác thực | 

Các ràng buộc n 2600 cho phép lên tới khoảng 6,7 triệu thao tác trên mỗi chu kỳ kiểm tra cặp ứng cử viên theo cách hiểu tồi tệ nhất, nhưng trên thực tế, việc từ chối sớm giúp giải pháp đủ hiệu quả trong Python với hàm băm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = []
    
    def input():
        return sys.stdin.readline()
    
    n = int(sys.stdin.readline())
    pts = [tuple(map(int, sys.stdin.readline().split())) for _ in range(2*n)]
    
    from collections import Counter

    def try_sum(Sx, Sy):
        cnt = Counter(pts)
        for x, y in pts:
            if cnt[(x, y)] == 0:
                continue
            cnt[(x, y)] -= 1
            tx, ty = Sx - x, Sy - y
            if cnt[(tx, ty)] == 0:
                cnt[(x, y)] += 1
                return None
            cnt[(tx, ty)] -= 1
        return (Sx//2, Sy//2)

    for i in range(1, 2*n):
        sx = pts[0][0] + pts[i][0]
        sy = pts[0][1] + pts[i][1]
        res = try_sum(sx, sy)
        if res is not None and res not in pts:
            return f"Yes\n{res[0]} {res[1]}\n"

    return "No\n"

# sample
assert run("""2
1 1
1 3
3 1
3 3
""") == "Yes\n2 2\n"

# collinear symmetric
assert run("""2
0 0
0 2
2 0
2 2
""") == "Yes\n1 1\n"

# impossible case
assert run("""1
0 0
1 1
""") == "No\n"

# degenerate valid
assert run("""1
-1 -1
1 1
""") == "Yes\n0 0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1,1,1,3,3,1,3,3 | Có 2 2 | đối xứng vuông cơ bản | 
| 0,0,0,2,2,0,2,2 | Có 1 1 | nhiều cặp hợp lệ | 
| 0,0,1,1 | Không | không có tâm số nguyên | 
| -1,-1,1,1 | Có 0 0 | xử lý tọa độ âm | 

## Vỏ cạnh 

Một dạng thất bại là khi tồn tại nhiều trung tâm ứng cử viên do tính đối xứng một phần nhưng chỉ có một trung tâm cho phép ghép đôi hoàn chỉnh. Thuật toán thử tất cả các cặp liên quan đến một điểm neo cố định, do đó, mọi cấu hình hợp lệ đều phải bao gồm tổng ứng cử viên chính xác trong số những lần thử đó. Nếu thử một trung tâm đối xứng nhưng không hợp lệ, việc ghép nối tham lam sẽ thất bại ở phần bù chưa từng có đầu tiên, ngăn chặn việc chấp nhận sai. 

Một trường hợp khác là khi một điểm là phần bù của chính nó dưới tổng ứng cử viên, nghĩa là S = 2P. Việc triển khai xử lý vấn đề này một cách chính xác vì số điểm bị giảm trước khi kiểm tra đối tác của nó, đảm bảo rằng việc tự ghép nối chỉ thành công khi tồn tại ít nhất hai điểm giống nhau, điều này là không thể vì tất cả các điểm đều khác nhau. 

Cuối cùng, các trường hợp tâm thực nằm bên ngoài bao lồi được xử lý một cách tự nhiên vì việc tính toán không dựa vào các giả định hình học về vị trí mà chỉ dựa vào tính nhất quán của cặp đại số.
