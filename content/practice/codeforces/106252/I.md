---
title: "CF 106252I - Trình mô phỏng tình nguyện"
description: "Chúng tôi xử lý luồng nội dung gửi được chấp nhận theo trình tự thời gian trong một cuộc thi lập trình. Mỗi bài nộp thuộc về một nhóm và một vấn đề, đồng thời tất cả các bài nộp đều là những bài nộp thành công, vì vậy mỗi dòng thể hiện một nỗ lực giải pháp chính xác."
date: "2026-06-19T14:17:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106252
codeforces_index: "I"
codeforces_contest_name: "The 2025 ICPC Asia Shenyang Regional Contest (The 4th Universal Cup. Stage 6: Grand Prix of Shenyang)"
rating: 0
weight: 106252
solve_time_s: 52
verified: true
draft: false
---

[CF 106252I - Trình mô phỏng tình nguyện viên](https://codeforces.com/problemset/problem/106252/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi xử lý luồng nội dung gửi được chấp nhận theo trình tự thời gian trong một cuộc thi lập trình. Mỗi bài nộp thuộc về một nhóm và một vấn đề, đồng thời tất cả các bài nộp đều là những bài nộp thành công, vì vậy mỗi dòng thể hiện một nỗ lực giải pháp chính xác. 

Đối với mỗi lần gửi, chúng tôi phải quyết định xem liệu một quả bóng có được trao hay không. Một quả bóng chỉ được trao khi một đội giải quyết được vấn đề lần đầu tiên. Tuy nhiên, vấn đề thời gian. Trước thời gian kết thúc cuộc thi là 240 phút, mỗi đội giải được một bài toán đầu tiên luôn mang lại một quả bóng bay. Sau khi đóng băng, giải quyết đầu tiên chỉ mang lại một quả bóng bay nếu đội đó nhận được tổng cộng ít hơn ba quả bóng bay cho đến nay. 

Vì vậy, nhiệm vụ không chỉ là theo dõi các giải pháp đầu tiên của mỗi đội và vấn đề mà còn duy trì số lượng bóng bay mà mỗi đội đã nhận được và áp dụng quy tắc có điều kiện tùy thuộc vào thời gian gửi. 

Kích thước đầu vào tối đa là 5000 bài gửi, với tối đa 410 đội và 13 bài toán. Kích thước này đủ nhỏ để chúng ta có thể thoải mái duy trì trạng thái của từng nhóm trong mảng. Bất kỳ giải pháp nào kiểm tra các lần gửi trước đó một cách đơn giản cho mỗi truy vấn sẽ chỉ quá chậm nếu được triển khai không hiệu quả, nhưng ngay cả việc quét bậc hai trên 5000 vẫn có thể chấp nhận được; mối quan tâm thực sự là theo dõi trạng thái sạch hơn là giới hạn tiệm cận. 

Một trường hợp thất bại tinh tế xuất phát từ các bài gửi được chấp nhận trùng lặp cho cùng một nhóm và một vấn đề. Chỉ bài gửi được chấp nhận đầu tiên mới quan trọng đối với logic bong bóng. 

Ví dụ: nếu một đội giải quyết vấn đề 2 ở thời điểm 10 và một lần nữa ở thời điểm 50 thì chỉ nên xem xét vấn đề đầu tiên. Một cách tiếp cận ngây thơ không theo dõi các trạng thái đã giải quyết sẽ trao thưởng không chính xác nhiều quả bóng bay. 

Một trường hợp khác là sau thời gian đóng băng. Giả sử một đội đã có ba quả bóng bay trước thời gian 240, sau đó giải một bài toán mới sau 240. Mặc dù đây là lần giải đầu tiên nhưng không có quả bóng bay nào được đưa ra. Việc thực hiện bất cẩn vẫn có thể mang lại kết quả vì đây là giải pháp đầu tiên. 

Cuối cùng, việc sắp xếp thứ tự rất quan trọng vì các bài nộp đã được sắp xếp theo thời gian. Điều này đảm bảo chúng tôi có thể xử lý tuần tự và duy trì trạng thái gia tăng chính xác mà không cần sắp xếp lại hoặc xem trước. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý từng lần gửi một cách độc lập và đối với mỗi lần gửi, hãy quét tất cả các lần gửi trước đó để xác định xem đây có phải là lần đầu tiên nhóm giải quyết vấn đề đó hay không. Nếu đúng như vậy, chúng tôi sẽ đếm xem nhóm đã nhận được bao nhiêu quả bóng bay cho đến nay bằng cách quét các kết quả đầu ra trước đó. Điều này hiệu quả vì nó tái tạo lại tất cả lịch sử cần thiết ở mỗi bước một cách rõ ràng. 

Tuy nhiên, đối với mỗi lần gửi trong số n lần gửi, việc này yêu cầu phải quét tối đa n mục trước đó, dẫn đến thời gian là O(n^2). Với n lên tới 5000, con số này là khoảng 25 triệu séc, đây là mức giới hạn nhưng không cần thiết do có cấu trúc đơn giản hơn. Quan trọng hơn, cách tiếp cận này khó thực hiện chính xác vì nó kết hợp việc tính toán lại các trạng thái đã giải quyết và số lượng bong bóng lặp đi lặp lại. 

Quan sát quan trọng là chúng ta không bao giờ cần tính toán lại lịch sử. Chúng ta chỉ cần biết hai điều vào bất kỳ lúc nào: liệu một đội đã giải quyết được một vấn đề nhất định chưa và đội đó đã nhận được bao nhiêu quả bóng bay. Cả hai có thể được lưu trữ tăng dần. 

Chúng tôi duy trì trạng thái boolean cho mỗi nhóm và cặp vấn đề cho biết liệu vấn đề đã được nhóm đó giải quyết hay chưa. Cho đến nay, chúng tôi cũng duy trì một quầy đếm bóng bay được trao cho mỗi đội. Sau đó, mỗi lần gửi được xử lý theo thời gian liên tục bằng cách kiểm tra và cập nhật các cấu trúc này. 

Điều này làm giảm toàn bộ vấn đề thành mô phỏng phát trực tuyến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm và không cần thiết | 
| Tối ưu | O(n) | O(đội × vấn đề) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng hồ sơ một theo thứ tự. 

1. Khởi tạo mảng boolean 2D`solved[team][problem]`là sai đối với tất cả các đội và các vấn đề. Điều này theo dõi xem một nhóm đã giải quyết được một vấn đề nhất định trước đó hay chưa. 
2. Khởi tạo một mảng`balloons[team] = 0`cho tất cả các đội. Điều này lưu trữ số lượng bóng bay mà mỗi đội đã nhận được cho đến nay. 
3. Đối với mỗi lần gửi`(a, b, c)`theo thứ tự, trước tiên hãy kiểm tra xem`solved[a][b]`đã đúng rồi. Nếu đúng, xuất 0 ngay lập tức vì đây là phép giải lặp lại và không tạo ra bong bóng. 
4. Nếu sai thì đánh dấu`solved[a][b] = true`vì đây là lần đầu tiên nhóm giải quyết được vấn đề này. 
5. Nếu thời gian nộp hồ sơ`c < 240`, chúng tôi luôn trao tặng một quả bóng bay, vì vậy chúng tôi tăng dần`balloons[a]`và xuất id vấn đề`b`. 
6. Nếu thời gian nộp hồ sơ`c >= 240`, chúng tôi kiểm tra`balloons[a] < 3`. Nếu đúng thì chúng ta tăng`balloons[a]`và đầu ra`b`. Ngược lại chúng ta xuất ra 0. 

Lựa chọn thiết kế quan trọng là chúng tôi cập nhật trạng thái đã giải quyết ngay trong lần gặp đầu tiên, đảm bảo tất cả các lần gửi trong tương lai cho vấn đề đó sẽ bị bỏ qua bất kể thời gian. 

Tại sao nó hoạt động: tại bất kỳ thời điểm nào trong quá trình xử lý luồng,`solved[a][b]`thể hiện chính xác liệu sự kiện giải quyết đầu tiên cho cặp đó đã xảy ra trước đó trong chuỗi hay chưa. Vì chúng tôi xử lý theo thứ tự thời gian nên trạng thái này nhất quán với diễn biến cuộc thi thực tế. các`balloons[a]`bộ đếm phản ánh chính xác số lần giải quyết đầu tiên được chấp nhận trước đó và thỏa mãn quy tắc trao giải, do đó, điều kiện đóng băng có thể được đánh giá cục bộ mà không cần tính toán lại lịch sử. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    solved = [[False] * 14 for _ in range(411)]
    balloons = [0] * 411
    
    out = []
    
    for _ in range(n):
        a, b, c = map(int, input().split())
        
        if solved[a][b]:
            out.append("0")
            continue
        
        solved[a][b] = True
        
        if c < 240:
            balloons[a] += 1
            out.append(str(b))
        else:
            if balloons[a] < 3:
                balloons[a] += 1
                out.append(str(b))
            else:
                out.append("0")
    
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp máy trạng thái được mô tả trước đó. các`solved`bảng được định cỡ bằng cách sử dụng các ràng buộc của bài toán: tối đa 410 nhóm và 13 bài toán, vì vậy chúng tôi phân bổ nhiều hơn một chút để tránh các vấn đề riêng lẻ khi lập chỉ mục. các`balloons`mảng theo dõi tổng số của mỗi đội. 

Một điểm tinh tế là chúng tôi đánh dấu`solved[a][b] = True`ngay lập tức khi chúng tôi phát hiện lời giải đầu tiên, trước khi áp dụng logic thời gian. Điều này đảm bảo việc gửi đi lặp lại không bao giờ vô tình nhập lại logic trao giải ngay cả khi điều kiện thời gian khác nhau. 

Chúng tôi cũng tích lũy kết quả đầu ra vào một danh sách và in một lần, điều này tránh được chi phí I/O lặp lại. 

## Ví dụ đã hoạt động 

Hãy xem xét kịch bản đơn giản hóa sau đây: 

đầu vào:```
1 1 100
1 2 200
1 1 250
1 3 260
```| Bước | Đội | Vấn đề | Thời gian | Giải Quyết Trước | Bóng bay | Hành động | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 100 | Không | 0 | Giải thưởng | 1 | 
| 2 | 1 | 2 | 200 | Không | 1 | Giải thưởng | 2 | 
| 3 | 1 | 1 | 250 | Có | 2 | Bỏ qua | 0 | 
| 4 | 1 | 3 | 260 | Không | 2 | <3 vậy giải thưởng | 3 | 

Dấu vết này cho thấy cả hai hành vi quan trọng: các giải pháp lặp lại sẽ bị bỏ qua ngay lập tức và logic sau khi đóng băng vẫn phụ thuộc vào số lượng bong bóng tích lũy. 

Bây giờ hãy xem xét một kịch bản giới hạn đóng băng: 

đầu vào:```
1 1 10
1 2 20
1 3 30
1 4 250
1 5 260
```| Bước | Đội | Vấn đề | Thời gian | Giải Quyết Trước | Bóng bay | Hành động | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 10 | Không | 0 | Giải thưởng | 1 | 
| 2 | 1 | 2 | 20 | Không | 1 | Giải thưởng | 2 | 
| 3 | 1 | 3 | 30 | Không | 2 | Giải thưởng | 3 | 
| 4 | 1 | 4 | 250 | Không | 3 | Không có giải thưởng (giới hạn) | 0 | 
| 5 | 1 | 5 | 260 | Không | 3 | Vẫn bị chặn | 0 | 

Điều này xác nhận rằng quy tắc đóng băng chỉ phụ thuộc vào bong bóng tích lũy chứ không phụ thuộc vào số lượng vấn đề mới được giải quyết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi lần gửi sẽ kích hoạt kiểm tra và cập nhật liên tục trên mảng | 
| Không gian | O(đội × vấn đề) | Bảng Boolean lưu trữ trạng thái đã giải cộng với bộ đếm cho mỗi đội | 

Các ràng buộc đủ nhỏ để mô phỏng trực tiếp có thể dễ dàng thực hiện nhanh chóng. Với tối đa 5000 lần gửi, ngay cả Python cũng xử lý việc này một cách thoải mái vì mỗi thao tác là O(1). 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n = int(sys.stdin.readline())
    solved = [[False] * 14 for _ in range(411)]
    balloons = [0] * 411
    out = []
    
    for _ in range(n):
        a, b, c = map(int, sys.stdin.readline().split())
        if solved[a][b]:
            out.append("0")
            continue
        solved[a][b] = True
        if c < 240:
            balloons[a] += 1
            out.append(str(b))
        else:
            if balloons[a] < 3:
                balloons[a] += 1
                out.append(str(b))
            else:
                out.append("0")
    
    return "\n".join(out)

# sample-like cases
assert run("4\n1 1 10\n1 2 20\n1 1 250\n1 3 260") == "1\n2\n0\n3"

# minimum input
assert run("1\n1 1 0") == "1"

# repeated submissions only
assert run("3\n1 1 10\n1 1 20\n1 1 30") == "1\n0\n0"

# freeze boundary
assert run("4\n1 1 239\n1 2 240\n1 3 241\n1 4 242") == "1\n2\n3\n0"

# multiple teams independence
assert run("4\n1 1 10\n2 1 20\n1 2 30\n2 2 40") == "1\n1\n2\n2"

# post-freeze cap
assert run("5\n1 1 10\n1 2 20\n1 3 30\n1 4 250\n1 5 260") == "1\n2\n3\n0\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đầu vào tối thiểu | giải thưởng duy nhất | xử lý trường hợp cơ bản | 
| đệ trình lặp đi lặp lại | logic duy nhất | lọc trùng lặp | 
| ranh giới đóng băng | thời gian cắt chính xác | quy tắc 240 nghiêm ngặt | 
| nhiều đội | độc lập | tách trạng thái mỗi đội | 
| nắp sau đóng băng | giới hạn bóng | thực thi quy tắc <3 | 

## Vỏ cạnh 

Một trường hợp phức tạp được lặp lại sẽ giải quyết được ranh giới đóng băng. Giả sử một nhóm giải quyết được một vấn đề trước khi đóng băng và một lần nữa sau đó: 

đầu vào:```
1 1 100
1 1 250
```Bộ đệ trình đầu tiên`solved[1][1] = True`và trao một quả bóng bay. Lần gửi thứ hai ngay lập tức không vượt qua được bước kiểm tra đã giải quyết và đưa ra kết quả là 0. Quy tắc đóng băng thậm chí không bao giờ được xem xét vì điều kiện giải quyết lần đầu đã bị phá vỡ, phù hợp với định nghĩa vấn đề. 

Một trường hợp cạnh khác là đạt đến giới hạn bong bóng chính xác vào thời điểm đóng băng:```
1 1 10
1 2 20
1 3 30
1 4 240
```Sau ba lần giải quyết đóng băng trước,`balloons[1] = 3`. Lần nộp thứ tư xảy ra đúng lúc 240, do đó nó đang ở chế độ hậu đóng băng. Vì điều kiện yêu cầu ít hơn 3 quả bóng bay nên không có quả bóng nào được trao. Việc kiểm tra việc thực hiện`balloons[a] < 3`, do đó đầu ra chính xác là 0.
