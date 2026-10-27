---
title: "CF 106124K - km/h"
description: "Chúng tôi đang mô phỏng một người lái xe di chuyển qua một chuỗi biển báo đường, trong đó mỗi biển báo đặt ra giới hạn tốc độ cụ thể hoặc loại bỏ hạn chế hiện tại và khôi phục giới hạn tốc độ quốc gia ban đầu."
date: "2026-06-20T05:32:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106124
codeforces_index: "K"
codeforces_contest_name: "2025-2026 ICPC Nordic Collegiate Programming Contest (NCPC 2025)"
rating: 0
weight: 106124
solve_time_s: 41
verified: true
draft: false
---

[CF 106124K - km/h](https://codeforces.com/problemset/problem/106124/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một người lái xe di chuyển qua một chuỗi biển báo đường, trong đó mỗi biển báo đặt ra giới hạn tốc độ cụ thể hoặc loại bỏ hạn chế hiện tại và khôi phục giới hạn tốc độ quốc gia ban đầu. 

Chi tiết quan trọng là chúng tôi chưa biết giới hạn tốc độ quốc gia, ngoại trừ việc nó là bội số của 10 và cao hơn bất kỳ biển báo giới hạn tốc độ rõ ràng nào từng được hiển thị. Điều này có nghĩa là bất cứ khi nào chúng tôi nhìn thấy dấu hiệu “đặt lại”, chúng tôi không thể trực tiếp khôi phục một số cố định. Thay vào đó, chúng ta phải suy ra tốc độ tối đa có thể phù hợp với mọi thứ được quan sát cho đến nay. 

Nhiệm vụ là tuần tự. Sau mỗi biển báo, chúng tôi phải xuất ra tốc độ cao nhất được đảm bảo hợp pháp với tất cả các ràng buộc cho đến thời điểm đó. 

Đầu vào là một luồng hoạt động. Mỗi thao tác là một giới hạn số, ngay lập tức giới hạn tốc độ tối đa được phép ở giá trị đó hoặc biểu tượng gạch chéo biểu thị việc đặt lại, loại bỏ tất cả các giới hạn rõ ràng và đưa chúng ta trở lại chế độ giới hạn tốc độ quốc gia chưa xác định. 

Các ràng buộc là nhỏ, tối đa là 100 dấu hiệu. Điều này có nghĩa là mọi lời giải theo thời gian bậc hai đều đã an toàn, nhưng cấu trúc của bài toán đủ đơn giản để mô phỏng tuyến tính là đủ. 

Một trường hợp lỗi nhỏ xuất hiện xung quanh việc đặt lại. Nếu một người giả định không chính xác giới hạn tốc độ quốc gia là cố định hoặc cố gắng “ghi nhớ” một giá trị đoán được thì logic sẽ bị phá vỡ. 

Ví dụ, hãy xem xét: 

đầu vào:```
/
```Điều này là không thể vì biển báo đầu tiên phải là giới hạn tốc độ, nhưng nó làm nổi bật vấn đề: sau khi thiết lập lại, chúng tôi không có con số cụ thể trừ khi chúng tôi lấy nó từ giới hạn trước đó. 

Một trường hợp cạnh thực tế hơn là: 

đầu vào:```
50
80
/
```Lập luận đúng: 

Sau 50, giới hạn là 50. 

Sau 80, giới hạn trở thành 50 (vì chúng tôi luôn tuân theo giới hạn nhỏ nhất từng thấy cho đến nay). 

Sau khi đặt lại, giới hạn này trở thành giới hạn tốc độ quốc gia chưa xác định, giới hạn này phải lớn hơn bất kỳ giới hạn nào được thấy, ít nhất là 90 (vì nó phải là bội số của mười). Do đó, câu trả lời trở thành 90. 

Cách triển khai đơn giản chỉ đơn giản là "quên các ràng buộc" và in giá trị không giới hạn hoặc số 0 sau khi đặt lại là không chính xác. 

Khó khăn cốt lõi là việc đặt lại yêu cầu phải xây dựng lại giới hạn trên nhất quán từ lịch sử chứ không chỉ đơn thuần là xóa trạng thái. 

## Phương pháp tiếp cận 

Giải thích bạo lực sẽ cố gắng tính lại tốc độ tối đa hợp lệ sau mỗi dấu hiệu bằng cách xem xét lại tất cả các ràng buộc trước đó. Sau mỗi lần đặt lại, chúng tôi sẽ quét ngược để tìm giới hạn tốc độ tối đa được thấy cho đến nay và sau đó giả sử giới hạn tốc độ quốc gia là bội số nhỏ nhất của 10 lớn hơn giới hạn đó. Điều này có tác dụng về mặt khái niệm vì giới hạn quốc gia phải nhất quán với tất cả các giới hạn trước đó. 

Tuy nhiên, cách tiếp cận này trở nên không hiệu quả nếu mở rộng sang đầu vào lớn, vì mỗi lần đặt lại có thể kích hoạt quét toàn bộ tất cả các dấu hiệu trước đó, dẫn đến hành vi bậc hai trong trường hợp xấu nhất. Mặc dù N ở đây nhỏ nhưng điều này thúc đẩy việc đơn giản hóa trạng thái. 

Quan sát quan trọng là chúng ta không bao giờ cần phải tính toán lại từ đầu. Thông tin duy nhất quan trọng là giới hạn tốc độ tối đa được thấy kể từ lần đặt lại gần đây nhất. Chỉ riêng giá trị đó đã xác định giới hạn tốc độ quốc gia tối thiểu có thể có: bội số nhỏ nhất của mười lớn hơn nó. Do đó, thay vì quét lịch sử, chúng tôi duy trì mức hoạt động tối đa được đặt lại bất cứ khi nào dấu gạch chéo xuất hiện. 

Điều này làm giảm toàn bộ quá trình thành một lần duy nhất với các cập nhật liên tục theo thời gian cho mỗi dấu hiệu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^2) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một phần trạng thái: giới hạn tốc độ rõ ràng cao nhất được thấy kể từ lần đặt lại gần đây nhất. 

1. Khởi tạo một biến`mx`về 0. Điều này thể hiện ràng buộc mạnh nhất hiện đang hoạt động. 
2. Đọc ký hiệu đầu tiên được đảm bảo là số và đặt`mx`đến giá trị đó. Xuất nó ngay lập tức. 
3. Đối với mỗi dấu hiệu tiếp theo, hãy xử lý theo thứ tự. 
4. Nếu dấu hiệu là số`L`, cập nhật`mx`được`max(mx, L)`. đầu ra`mx`bởi vì tốc độ hợp pháp luôn bị giới hạn bởi những hạn chế chặt chẽ nhất từ ​​trước đến nay. 
5. Nếu dấu hiệu là`/`, hãy tính giới hạn tốc độ quốc gia là bội số nhỏ nhất của mười lớn hơn chính xác`mx`. Điều này có thể được thực hiện như`((mx // 10) + 1) * 10`. Xuất giá trị này, nhưng quan trọng là không đặt lại`mx`về không; thay vào đó, về mặt khái niệm, chúng tôi chuyển sang chế độ “không hạn chế ngoại trừ giới hạn quốc gia”, có nghĩa là các giới hạn rõ ràng trong tương lai sẽ thiết lập lại các ràng buộc bắt đầu từ đường cơ sở đó. 
6. Tiếp tục cho đến khi tất cả các dấu hiệu được xử lý. 

Ý tưởng chính là`mx`luôn theo dõi ràng buộc rõ ràng mạnh nhất trong phân đoạn hiện tại giữa các lần đặt lại. Dấu gạch chéo không xóa lịch sử vì mục đích chính xác mà chỉ thay đổi cách chúng ta diễn giải phân đoạn tiếp theo. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào không có thiết lập lại, tính hợp pháp sẽ bị chi phối hoàn toàn bởi mức tối thiểu trên tất cả các giới hạn tốc độ được thấy kể từ lần đặt lại cuối cùng. Mức tối thiểu đó chính xác là mức hiện tại`mx`. Khi thiết lập lại xảy ra, giá trị còn thiếu duy nhất là giới hạn tốc độ quốc gia, nhưng vấn đề đảm bảo rằng nó là bội số của 10, vượt quá mọi giới hạn được quan sát. Giá trị nhỏ nhất đó hoàn toàn được xác định bởi`mx`, vì vậy chúng ta có thể xuất nó một cách an toàn mà không cần đoán bất cứ điều gì khác. Không có thông tin lịch sử nào khác ảnh hưởng đến câu trả lời hiện tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    
    mx = 0
    first = True
    
    for _ in range(n):
        s = input().strip()
        
        if s == "/":
            # restore national speed limit
            mx = ((mx // 10) + 1) * 10
            print(mx)
        else:
            val = int(s)
            if first:
                mx = val
                first = False
            else:
                mx = max(mx, val)
            print(mx)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì hoạt động tối đa`mx`đại diện cho hạn chế rõ ràng chặt chẽ nhất trong phân khúc hiện tại. Khi gặp ký hiệu số, chúng tôi cập nhật mức tối đa này. Khi gặp dấu gạch chéo, chúng tôi chuyển mức tối đa hiện tại thành giới hạn quốc gia ngụ ý bằng cách làm tròn lên bội số tiếp theo của 10. 

các`first`cờ đảm bảo rằng trạng thái ban đầu được xử lý rõ ràng, vì vấn đề đảm bảo biển báo đầu tiên luôn là giới hạn tốc độ. 

Một cạm bẫy phổ biến là việc đặt lại`mx`về 0 trên`/`. Điều đó có nghĩa không chính xác là không có hạn chế, trong khi hành vi đúng là suy ra một giới hạn cụ thể từ mức tối đa trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
50
80
/
60
/
```Chúng tôi theo dõi`mx`và xuất ra từng bước. 

| Bước | Ký tên | mx trước | Hành động | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 50 | 0 | mx = 50 | 50 | 
| 2 | 80 | 50 | mx = 80 | 80 | 
| 3 | / | 80 | làm tròn lên 90 | 90 | 
| 4 | 60 | 90 | mx = max(90, 60) = 90 | 90 | 
| 5 | / | 90 | làm tròn lên 100 | 100 | 

Dấu vết này cho thấy rằng sau khi thiết lập lại, các ràng buộc rõ ràng trước đó vẫn ảnh hưởng đến giới hạn quốc gia được suy ra thông qua mức tối đa được thực hiện. 

### Ví dụ 2 

đầu vào:```
40
30
/
70
/
```| Bước | Ký tên | mx trước | Hành động | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 40 | 0 | mx = 40 | 40 | 
| 2 | 30 | 40 | mx ở lại 40 | 40 | 
| 3 | / | 40 | làm tròn lên 50 | 50 | 
| 4 | 70 | 50 | mx = 70 | 70 | 
| 5 | / | 70 | làm tròn lên 80 | 80 | 

Điều này xác nhận rằng thuật toán hợp nhất chính xác các ràng buộc giữa các phân đoạn được phân tách bằng các lần đặt lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi dấu hiệu được xử lý một lần với các bản cập nhật liên tục | 
| Không gian | O(1) | Chỉ một lần chạy tối đa duy nhất được lưu trữ | 

Với N ≤ 100, tốc độ này nhanh không đáng kể, nhưng cấu trúc tuyến tính phản ánh bản chất thực sự của quy trình và mở rộng quy mô thành đầu vào lớn hơn nhiều mà không cần sửa đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample-like tests
assert run("1\n50\n") == "50"

assert run("3\n50\n80\n/\n") == "50\n80\n90"

# custom cases

# minimum case
assert run("1\n10\n") == "10", "single limit"

# all increasing limits
assert run("4\n10\n20\n30\n40\n") == "10\n20\n30\n40", "monotonic increase"

# reset after peak
assert run("5\n10\n90\n/\n50\n/\n") == "10\n90\n100\n100\n110", "reset behavior"

# repeated resets
assert run("5\n30\n/\n/\n20\n/\n") == "30\n40\n50\n50\n60", "multiple resets"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 10 | 10 | trường hợp cơ sở | 
| trình tự tăng dần | đầu ra đơn điệu | cập nhật bình thường | 
| thiết lập lại sau cao điểm | suy luận tròn | logic tốc độ quốc gia | 
| đặt lại nhiều lần | ổn định qua các chu kỳ | suy luận lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi mức tối đa trước khi đặt lại đã là bội số của mười. Ví dụ: 

đầu vào:```
60
/
```Chúng tôi tính toán`((60 // 10) + 1) * 10 = 70`, do đó đầu ra trở thành 70. Một cách tiếp cận đơn giản chỉ trả về 60 sau khi đặt lại sẽ vi phạm yêu cầu rằng giới hạn tốc độ quốc gia phải cao hơn bất kỳ giới hạn rõ ràng nào được thấy cho đến nay. 

Một trường hợp khác là khi đặt lại các giới hạn nhỏ: 

đầu vào:```
10
20
/
```Chúng tôi theo dõi`mx = 20`, sau đó đặt lại đầu ra 30. Điều này cho thấy việc đặt lại không trở lại một hằng số cố định mà phụ thuộc hoàn toàn vào lịch sử tích lũy. 

Cuối cùng, việc đặt lại nhiều lần mà không có giới hạn mới sẽ xác nhận tính ổn định: 

đầu vào:```
10
/
/
```Bước 1 cho 10, bước 2 cho 20, bước 3 cho 30. Mỗi lần đặt lại tiếp tục rút ra một chuỗi tăng dần nghiêm ngặt, phù hợp với quy luật giới hạn quốc gia luôn cao hơn giới hạn quan sát trước đó.
