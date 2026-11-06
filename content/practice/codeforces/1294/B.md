---
problem: 1294B
contest_id: 1294
problem_index: B
name: "Collecting Packages"
contest_name: "Codeforces Round 615 (Div. 3)"
rating: 1200
tags: ["implementation", "sortings"]
answer: passed_samples
verified: true
solve_time_s: 260
date: 2026-06-14
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2dc44f-5540-83ec-8334-913517db904d
---

#CF 1294B - Thu thập các gói hàng 

**Đánh giá:** 1200 
**Tags:** triển khai, sắp xếp 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 4 phút 20s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2dc44f-5540-83ec-8334-913517db904d 

--- 

##Giải pháp 

## Hiểu vấn đề 

Robot bắt đầu ở điểm gốc của lưới và chỉ có thể di chuyển theo hai hướng: sang phải hoặc lên trên. Mỗi gói được đặt tại tọa độ lưới riêng biệt với các giá trị nguyên không âm. Robot phải đến thăm tất cả các vị trí đóng gói bắt đầu từ điểm gốc và được phép tự do lựa chọn thứ tự tham quan. Mỗi lần di chuyển sẽ tăng tọa độ x hoặc tọa độ y thêm một. 

Nhiệm vụ là xác định xem có tồn tại thứ tự ghé thăm tất cả các điểm sao cho robot không bao giờ cần di chuyển sang trái hoặc xuống trong khi vẫn có thể tiếp cận mọi gói hàng theo thứ tự đó hay không. Nếu thứ tự như vậy tồn tại, chúng ta phải xây dựng một chuỗi các bước đi ngắn nhất hợp lệ. Trong số tất cả các đường dẫn hợp lệ ngắn nhất, chúng ta phải xuất ra đường dẫn nhỏ nhất về mặt từ điển, trong đó 'R' được coi là nhỏ hơn 'U'. 

Các ràng buộc làm cho giải pháp bị ràng buộc rất chặt chẽ với logic sắp xếp tuyến tính hoặc gần tuyến tính. Tổng số điểm trên tất cả các trường hợp thử nghiệm nhiều nhất là 1000, do đó, chiến lược sắp xếp O(n log n) là đủ dễ dàng. Bất cứ điều gì liên quan đến hoán vị hoặc quay lui trên tất cả các đơn hàng sẽ bùng nổ về mặt tổ hợp và ngay lập tức không thể thực hiện được nếu vượt quá n rất nhỏ. 

Một vấn đề tế nhị xuất hiện khi các điểm “không tương thích” về mặt truyền tải đơn điệu. Ví dụ: nếu chúng ta có các điểm (1, 2) và (2, 1), không có đường đi đơn điệu nào có thể đi qua cả hai theo một chuỗi chuyển động x và y tăng dần mà không vi phạm tính đơn điệu. Bất kỳ nỗ lực nào để buộc ra lệnh cuối cùng sẽ yêu cầu di chuyển xuống dưới hoặc sang trái, điều này không được phép. 

Một sai lầm ngây thơ là cố gắng sắp xếp các điểm chỉ theo x hoặc chỉ theo y. Ví dụ: chỉ sắp xếp theo x có thể tạo ra một chuỗi trong đó y giảm giữa các điểm liên tiếp, khiến cho việc truyền tải đơn điệu trực tiếp là không thể. Tương tự, việc sắp xếp các điểm tùy ý sẽ dẫn đến trạng thái trung gian không hợp lệ mà robot sẽ cần phải “quay lại”. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các hoán vị của các điểm, kiểm tra xem mỗi thứ tự có thể được duyệt qua chỉ bằng cách di chuyển sang phải và lên hay không, đồng thời theo dõi đường dẫn nhỏ nhất về mặt từ điển tốt nhất trong số các đường dẫn hợp lệ. Đối với mỗi hoán vị, chúng tôi mô phỏng việc di chuyển từ (0,0) đến điểm đầu tiên, sau đó giữa các điểm liên tiếp, xác minh rằng cả hai tọa độ không bao giờ giảm. Điều này hiệu quả vì nó thực thi tính khả thi một cách rõ ràng, nhưng nó yêu cầu kiểm tra n! đơn đặt hàng, và ngay cả với n = 10, điều này đã trở nên không khả thi về mặt tính toán. 

Quan sát cấu trúc quan trọng là bất kỳ đường dẫn hợp lệ nào cũng phải di chuyển theo kiểu x không giảm và y không giảm. Điều này có nghĩa là một khi chúng ta cam kết truy cập một điểm có x lớn hơn, thì sau này chúng ta không bao giờ có thể truy cập một điểm có x nhỏ hơn. Điều tương tự cũng áp dụng cho y trong các ràng buộc thứ tự tương đối giữa các điểm liên tiếp. 

Điều này gợi ý rằng nếu chúng ta sắp xếp các điểm theo tọa độ x và xử lý chúng theo thứ tự tăng dần, chúng ta sẽ tự động đảm bảo rằng chúng ta không bao giờ cần phải di chuyển sang trái. Vấn đề duy nhất còn lại là liệu chúng ta có thể duy trì tính khả thi trong phạm vi giá trị x bằng nhau hay không. Đối với đầu ra nhỏ nhất về mặt từ điển, chúng tôi muốn ưu tiên các bước di chuyển tạo ra 'R' càng sớm càng tốt, điều này phù hợp với việc ưu tiên các chuyển đổi x nhỏ hơn trước các chuyển đổi y. 

Tuy nhiên, cải tiến quan trọng nhất là chúng ta không thực sự cần phải thực thi thứ tự nghiêm ngặt giữa các điểm. Thay vào đó, chúng ta chỉ quan tâm rằng khi di chuyển từ vị trí hiện tại sang mục tiêu tiếp theo, chúng ta chỉ được di chuyển sang phải và lên trên, nghĩa là cả hai tọa độ phải không giảm dọc theo dãy đã chọn. Do đó, chiến lược tối ưu là sắp xếp các điểm theo x tăng dần và đối với x bằng nhau thì theo y tăng dần. Điều này đảm bảo chuỗi ngày càng đơn điệu ở cả hai chiều, đảm bảo tính khả thi.

Sau khi chuỗi này được cố định, việc xây dựng đường đi rất đơn giản: từ vị trí hiện tại, di chuyển sang phải cho đến khi đến x tiếp theo, sau đó đi lên cho đến khi đến y tiếp theo. Vì x không bao giờ giảm và y không bao giờ giảm nên điều này luôn đúng. 

Yêu cầu nhỏ nhất về mặt từ điển được thỏa mãn một cách tự nhiên vì chúng tôi luôn tận dụng hết tất cả các bước di chuyển 'R' cần thiết trước các bước di chuyển 'U' ở mỗi giai đoạn khi có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Sắp xếp theo tọa độ | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một đường dẫn hợp lệ bằng cách thực thi một tiến trình đơn điệu thông qua các điểm được sắp xếp. 

1. Đọc tất cả tọa độ gói cho trường hợp thử nghiệm. Chúng đại diện cho các ô mục tiêu mà robot phải tiếp cận. 
2. Sắp xếp tất cả các điểm bằng cách tăng tọa độ x và đối với x bằng nhau, bằng cách tăng tọa độ y. Điều này đảm bảo rằng khi di chuyển qua danh sách, cả hai tọa độ không bao giờ giảm, do đó không cần phải di chuyển lùi. 
3. Khởi tạo vị trí hiện tại tại (0, 0) và chuỗi kết quả trống. 
4. Lặp lại các điểm đã được sắp xếp. Đối với mỗi điểm (x, y), di chuyển theo chiều ngang từ x hiện tại đến x bằng các bước 'R', sau đó di chuyển theo chiều dọc từ y hiện tại đến y bằng các bước 'U'. Nối các bước di chuyển này vào chuỗi câu trả lời. Thứ tự này là bắt buộc vì việc di chuyển sang phải trước tiên sẽ duy trì tính tối thiểu về mặt từ điển. 
5. Sau khi xử lý tất cả các điểm, xuất ra đường dẫn đã xây dựng. 

Tại sao nó hoạt động 

Thứ tự được sắp xếp đảm bảo rằng cả x và y đều không giảm dọc theo chuỗi các điểm đã ghé thăm. Điều này đảm bảo rằng mọi chuyển đổi giữa các điểm liên tiếp đều có thể đạt được chỉ bằng cách di chuyển sang phải và lên. Vì chúng tôi luôn hoàn thành tất cả chuyển động ngang trước chuyển động dọc, nên chúng tôi tạo ra chuỗi nhỏ nhất có thể theo thứ tự từ điển trong số tất cả các đường dẫn đơn điệu hợp lệ, bởi vì bất kỳ nỗ lực nào để chèn chữ 'U' trước đó sẽ trì hoãn một 'R' cần thiết và tạo ra một tiền tố lớn hơn về mặt từ điển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]
    
    pts.sort()
    
    x, y = 0, 0
    res = []
    ok = True
    
    for nx, ny in pts:
        if nx < x or ny < y:
            ok = False
            break
        
        res.append('R' * (nx - x))
        res.append('U' * (ny - y))
        x, y = nx, ny
    
    if not ok:
        print("NO")
    else:
        print("YES")
        print(''.join(res))
```Mã bắt đầu bằng cách sắp xếp các điểm theo từ điển, tương ứng với việc sắp xếp theo x rồi y. Điều này đảm bảo tính chất đơn điệu cần thiết cho việc kiểm tra tính khả thi. Chúng tôi duy trì một con trỏ cho vị trí hiện tại của robot và dần dần xây dựng đường đi bằng cách cộng chính xác số lần di chuyển sang phải và lên cần thiết để đến từng điểm liên tiếp. 

Việc kiểm tra tính hợp lệ đảm bảo chúng tôi không bao giờ cố gắng lùi lại. Nếu bất kỳ điểm nào vi phạm tính đơn điệu so với điểm trước đó thì việc xây dựng sẽ thất bại. 

Thứ tự xây dựng rất quan trọng: chúng tôi luôn nối tất cả các bước di chuyển 'R' cần thiết trước các bước di chuyển 'U', điều này đảm bảo tính tối thiểu về mặt từ điển trong số các đường dẫn đơn điệu hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Điểm đầu vào: (1,3), (1,2), (3,3) 

Sau khi sắp xếp, ta được: (1,2), (1,3), (3,3) 

| Bước | Hiện tại (x,y) | Mục tiêu (x, y) | R di chuyển | bạn di chuyển | Đường dẫn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (0,0) | (1,2) | R | UU | RƯU | 
| 2 | (1,2) | (1,3) | | Bạn | RUUU | 
| 3 | (1,3) | (3,3) | RR | | RUUURR | 

Dấu vết này cho thấy rằng việc sắp xếp sẽ giải quyết đầu vào dường như “chưa được sắp xếp” thành một chuỗi đơn điệu. Robot không bao giờ cần giảm tọa độ, khẳng định tính khả thi. 

### Ví dụ 2 

Điểm đầu vào: (1,0), (0,1) 

Sau khi sắp xếp: (0,1), (1,0) 

| Bước | Hiện tại | Mục tiêu | Chuyển đổi hợp lệ | 
| --- | --- | --- | --- | 
| 1 | (0,0) | (0,1) | Bạn | 
| 2 | (0,1) | (1,0) | không hợp lệ (y giảm) | 

Điều này ngay lập tức thất bại vì y sẽ cần giảm từ 1 xuống 0. Thuật toán xuất ra chính xác NO. 

Điều này chứng tỏ rằng việc sắp xếp một mình là không đủ trừ khi tính khả thi đơn điệu được đảm bảo, điều mà việc kiểm tra thực thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế, mỗi bài kiểm tra xử lý điểm tuyến tính sau đó | 
| Không gian | O(n) | Lưu trữ tất cả các điểm và chuỗi đầu ra | 

Tổng số điểm trong tất cả các trường hợp thử nghiệm tối đa là 1000, do đó việc sắp xếp và xây dựng tuyến tính dễ dàng phù hợp với thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        pts = [tuple(map(int, input().split())) for _ in range(n)]
        pts.sort()

        x, y = 0, 0
        res = []
        ok = True

        for nx, ny in pts:
            if nx < x or ny < y:
                ok = False
                break
            res.append('R' * (nx - x))
            res.append('U' * (ny - y))
            x, y = nx, ny

        out.append("NO" if not ok else "YES\n" + ''.join(res))

    return "\n".join(out)

# provided samples
assert run("""3
5
1 3
1 2
3 3
5 5
4 3
2
1 0
0 1
1
4 3
""") == """YES
RUUURRRRUU
NO
YES
RRRRUUU"""

# minimum size
assert run("""1
1
0 0
""") == """YES
"""

# simple monotone chain
assert run("""1
3
0 1
1 1
1 2
""") == """YES
URRU"""

# impossible case
assert run("""1
2
1 0
0 1
""") == """NO"""

# large increasing chain
assert run("""1
3
0 0
1 0
2 0
""") == """YES
RR"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn (0,0) | CÓ | trường hợp cơ bản tầm thường | 
| chuỗi đơn điệu | CÓ URRU | thi công đúng trật tự | 
| (1,0),(0,1) | KHÔNG | phát hiện đặt hàng không khả thi | 
| dây chuyền ngang | CÓ RR | xử lý R thuần túy | 

## Vỏ cạnh 

Một gói duy nhất tại (0,0) được xử lý rõ ràng vì không cần di chuyển và trình tự được sắp xếp vẫn hợp lệ. Đầu ra chỉ đơn giản là CÓ với một đường dẫn trống, phù hợp với yêu cầu rằng robot đã có mặt ở vị trí đó. 

Hai điểm yêu cầu giao nhau theo hướng chéo ngược nhau, chẳng hạn như (1,0) và (0,1), sẽ thất bại ngay lập tức vì việc sắp xếp tạo ra một bước yêu cầu giảm tọa độ. Việc kiểm tra nắm bắt điều này trước khi xây dựng bất kỳ đường dẫn nào, đảm bảo tính chính xác. 

Các trường hợp tất cả các điểm nằm trên một đường tăng dần, chẳng hạn như (0,0), (1,0), (2,0), chứng tỏ rằng thuật toán suy biến thành một chuỗi thuần túy các bước di chuyển 'R' mà không có độ phức tạp không cần thiết, xác nhận tính tối ưu của việc xây dựng.
