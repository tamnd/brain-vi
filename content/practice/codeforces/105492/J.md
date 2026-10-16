---
title: "CF 105492J - Bảng điểm lộn xộn"
description: "Chúng tôi nhận được một chuỗi ảnh chụp nhanh từ bảng điểm trò chơi, trong đó mỗi ảnh chụp nhanh chứa hai số nguyên biểu thị điểm số của hai đội thi đấu tại thời điểm đó."
date: "2026-06-23T19:44:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105492
codeforces_index: "J"
codeforces_contest_name: "2024 Benelux Algorithm Programming Contest (BAPC 24)"
rating: 0
weight: 105492
solve_time_s: 55
verified: true
draft: false
---

[CF 105492J - Bảng điểm lộn xộn](https://codeforces.com/problemset/problem/105492/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được một chuỗi ảnh chụp nhanh từ bảng điểm trò chơi, trong đó mỗi ảnh chụp nhanh chứa hai số nguyên biểu thị điểm số của hai đội thi đấu tại thời điểm đó. Những ảnh chụp nhanh này được gửi theo thứ tự chúng đến, nhưng thứ tự đó có thể không phản ánh thứ tự thời gian thực tế mà trận đấu diễn ra. 

Nhiệm vụ là xác định xem chuỗi đã cho có thể biểu thị dòng thời gian hợp lệ của một trận đấu thực hay không. Dòng thời gian hợp lệ có nghĩa là từ ảnh chụp nhanh này sang ảnh chụp nhanh tiếp theo, điểm của cả hai đội đều không giảm. Cả hai điểm số có thể giữ nguyên hoặc tăng lên nhưng không bao giờ giảm theo thời gian. 

Vì vậy, chúng tôi không xây dựng lại dòng thời gian và chúng tôi không sắp xếp lại bất cứ thứ gì. Chúng ta chỉ đơn giản kiểm tra xem thứ tự đã cho có tuân thủ ràng buộc tính đơn điệu trên cả hai tọa độ cùng một lúc hay không. 

Các ràng buộc rất nhỏ: tối đa 100 ảnh chụp nhanh và mỗi điểm nằm trong khoảng từ 0 đến 100. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào thậm chí là bậc hai đều an toàn một cách thoải mái và quét tuyến tính là quá đủ. Không cần cấu trúc dữ liệu hoặc tìm kiếm phức tạp. 

Sự tinh tế chính trong vấn đề này là hiểu "thứ tự thời gian" nghĩa là gì. Đây không phải là việc sắp xếp theo thời gian một cách rõ ràng hoặc so sánh các dấu thời gian. Manh mối duy nhất chúng ta có là điểm số chỉ tăng theo thời gian. Quy tắc duy nhất đó xác định đầy đủ tính hợp lệ. 

Một số tình huống khó khăn có thể phá vỡ lối suy nghĩ ngây thơ: 

Nếu chúng ta chỉ kiểm tra xem tổng số điểm có tăng lên không, chúng ta có thể thất bại. Ví dụ: (2, 5) tiếp theo là (3, 4) có tổng là 7, tiếp theo là 7, nên tổng số không giảm nhưng điểm của đội thứ hai giảm từ 5 xuống 4 là không hợp lệ. Điều này sẽ được chấp nhận một cách không chính xác bởi một giải pháp bất cẩn. 

Nếu chúng tôi chỉ kiểm tra xem mỗi ảnh chụp nhanh có “lớn hơn hoặc bằng” theo một nghĩa nào đó như thứ tự tối đa hoặc tối thiểu hay không, thì chúng tôi cũng thất bại. Ví dụ: (1, 10) đến (2, 0) có cực đại tăng dần nhưng vi phạm tính đơn điệu trong cả hai thành phần. 

Vì vậy, điều kiện chính xác phải được kiểm tra theo từng thành phần giữa các ảnh chụp nhanh liên tiếp. 

## Phương pháp tiếp cận 

Tư duy bạo lực có thể cố gắng xác minh tính nhất quán bằng cách xem xét tất cả các cặp ảnh chụp nhanh và kiểm tra xem liệu có tồn tại một thứ tự nhất quán thỏa mãn các ràng buộc về tính đơn điệu trên toàn cầu hay không. Điều đó giống như việc xây dựng một biểu đồ thứ tự một phần và kiểm tra phần mở rộng tuyến tính hợp lệ. Mặc dù điều đó đúng về mặt lý thuyết nhưng nó hoàn toàn không cần thiết ở đây vì thứ tự đầu vào là cố định và chúng ta không được phép sắp xếp lại bất cứ thứ gì. 

Thay vào đó, chúng tôi quan sát thấy rằng nếu chuỗi hợp lệ theo thứ tự thời gian thì mọi chuyển đổi liền kề phải duy trì các giá trị không giảm cho cả hai đội. Ngược lại, nếu mọi cặp liền kề đều thỏa mãn điều kiện này thì toàn bộ chuỗi phải hợp lệ vì tính đơn điệu có tính bắc cầu: một khi không có bước nào giảm tọa độ thì không có ảnh chụp nhanh nào trước đó có thể vi phạm ảnh chụp nhanh sau đó. 

Điều này làm giảm vấn đề xuống còn một lần kiểm tra qua mảng, xác minh điều kiện bất đẳng thức đơn giản ở mỗi bước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (lý luận nhất quán toàn cầu) | O(n²) hoặc tệ hơn | O(n) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu (xác nhận liền kề) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các ảnh chụp nhanh theo thứ tự nhất định, lưu trữ chúng dưới dạng cặp số nguyên. 
2. Lặp lại từ ảnh chụp nhanh thứ hai đến ảnh chụp nhanh cuối cùng. 
3. Đối với mỗi ảnh chụp nhanh ở vị trí i, hãy so sánh nó với ảnh chụp nhanh trước đó ở vị trí i−1. 
4. Nếu điểm của một trong hai đội giảm từ i−1 xuống i, ngay lập tức kết luận chuỗi đó không hợp lệ. 
5. Nếu vòng lặp kết thúc mà không tìm thấy mức giảm nào thì kết luận dãy là hợp lệ. 

Mỗi bước tập trung vào tính kề cận vì bất kỳ sự vi phạm tính đơn điệu nào trước tiên đều phải xuất hiện ở một số chuyển tiếp ngay lập tức. Nếu xảy ra hiện tượng sụt giảm, nó không thể bị "ẩn" qua nhiều bước. 

### Tại sao nó hoạt động

Bất biến chính là sau khi xử lý từng vị trí i, tất cả các ảnh chụp nhanh từ 0 đến i đều nhất quán theo nghĩa là cả hai chuỗi điểm đều không giảm. Nếu tại bất kỳ thời điểm nào chúng tôi phát hiện thấy sự giảm, chúng tôi sẽ phá vỡ bất biến và loại bỏ chuỗi một cách chính xác. 

Nếu không có cặp liền kề nào vi phạm tính đơn điệu thì với bất kỳ chỉ số i < j nào, việc áp dụng lặp lại tính chất không giảm đảm bảo cả a[i] ≤ a[j] và b[i] ≤ b[j]. Điều này đảm bảo toàn bộ chuỗi có giá trị tạm thời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
prev_a, prev_b = map(int, input().split())

ok = True

for _ in range(n - 1):
    a, b = map(int, input().split())
    if a < prev_a or b < prev_b:
        ok = False
    prev_a, prev_b = a, b

print("yes" if ok else "no")
```Giải pháp chỉ giữ lại ảnh chụp nhanh trước đó trong khi quét tiếp, điều này là đủ vì tính hợp lệ chỉ phụ thuộc vào các lần chuyển đổi liên tiếp. Các biến`prev_a`Và`prev_b`đại diện cho trạng thái hợp lệ được xác nhận cuối cùng trong dòng thời gian. 

Kiểm tra có điều kiện`a < prev_a or b < prev_b`trực tiếp mã hóa quy tắc không được phép giảm điểm. Việc cập nhật các giá trị trước đó một cách vô điều kiện sẽ đảm bảo chúng tôi tiếp tục kiểm tra toàn bộ chuỗi ngay cả sau khi phát hiện vi phạm, mặc dù chúng tôi cũng có thể vi phạm sớm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 0
2 0
4 0
4 1
```| tôi | trước | hiện tại | chuyển đổi hợp lệ | 
| --- | --- | --- | --- | 
| 1 | (1,0) | (2,0) | vâng | 
| 2 | (2,0) | (4,0) | vâng | 
| 3 | (4,0) | (4,1) | vâng | 

Tất cả các chuyển đổi đều bảo toàn điểm số không giảm, vì vậy câu trả lời là có. 

Dấu vết này cho thấy rằng một khi cả hai chuỗi đều đơn điệu từng bước một thì toàn bộ dòng thời gian vẫn nhất quán. 

### Ví dụ 2 

đầu vào:```
3
0 0
1 0
0 2
```| tôi | trước | hiện tại | chuyển đổi hợp lệ | 
| --- | --- | --- | --- | 
| 1 | (0,0) | (1,0) | vâng | 
| 2 | (1,0) | (0,2) | không | 

Ở lần chuyển tiếp thứ hai, điểm của đội thứ nhất giảm từ 1 xuống 0, điều này ngay lập tức mất hiệu lực. 

Điều này chứng tỏ rằng ngay cả khi một tọa độ tăng lên đáng kể thì bất kỳ sự giảm nào ở tọa độ kia sẽ làm mất hiệu lực toàn bộ chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi xử lý mỗi ảnh chụp nhanh chính xác một lần và thực hiện so sánh theo thời gian liên tục | 
| Không gian | O(1) | Chỉ cặp điểm trước đó được lưu trữ | 

Các ràng buộc cho phép tối đa 100 ảnh chụp nhanh, nhưng ngay cả khi số lượng ảnh này lớn hơn nhiều thì quá trình quét tuyến tính vẫn hiệu quả. Giải pháp này là tối ưu vì mỗi giá trị đầu vào phải được đọc ít nhất một lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    prev_a, prev_b = map(int, input().split())

    ok = True

    for _ in range(n - 1):
        a, b = map(int, input().split())
        if a < prev_a or b < prev_b:
            ok = False
        prev_a, prev_b = a, b

    return "yes" if ok else "no"

# provided samples
assert run("4\n1 0\n2 0\n4 0\n4 1\n") == "yes"
assert run("3\n0 0\n1 0\n0 2\n") == "no"

# minimum size
assert run("1\n5 7\n") == "yes"

# monotone increase both
assert run("3\n0 0\n0 1\n2 3\n") == "yes"

# single drop in second coordinate
assert run("3\n0 0\n1 2\n2 1\n") == "no"

# flat sequence
assert run("4\n2 2\n2 2\n2 2\n2 2\n") == "yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 ảnh chụp nhanh | vâng | trường hợp cạnh tối thiểu | 
| tăng nghiêm ngặt | vâng | tiến triển hợp lệ bình thường | 
| thả tọa độ thứ hai | không | phát hiện giảm không hợp lệ | 
| giá trị không đổi | vâng | được phép bình đẳng | 

## Vỏ cạnh 

Đầu vào tối thiểu với một ảnh chụp nhanh duy nhất luôn thể hiện dòng thời gian hợp lệ vì không có trạng thái nào trước đó vi phạm tính đơn điệu. Thuật toán đọc cặp đầu tiên và ngay lập tức đưa ra kết quả có mà không cần vào vòng lặp. 

Một chuỗi phẳng trong đó tất cả các điểm vẫn giống hệt nhau trên các ảnh chụp nhanh cũng hợp lệ. Mỗi so sánh`a < prev_a or b < prev_b`đánh giá thành sai vì sự bình đẳng được cho phép. Bất biến giữ tầm thường ở mỗi bước. 

Một trường hợp thất bại khó phát hiện là khi một tọa độ tăng đáng kể trong khi tọa độ kia giảm nhẹ. Ví dụ: (1, 10) theo sau là (5, 3) có thể trông giống như tiến trình tổng thể, nhưng việc thả tọa độ thứ hai sẽ vô hiệu hóa nó ngay lập tức. Thuật toán nắm bắt được điều này ở lần chuyển đổi vi phạm đầu tiên và từ chối nó một cách chính xác.
