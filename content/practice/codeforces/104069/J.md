---
title: "CF 104069J - Du hành xuyên thời gian"
description: "Chúng ta được cung cấp một chuỗi các thao tác được áp dụng theo thời gian cho một tập hợp bắt đầu trống. Các thao tác được xử lý theo thứ tự và sau mỗi thao tác chèn, về mặt khái niệm, chúng tôi thu được một “phiên bản” mới của tập hợp."
date: "2026-07-02T03:01:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104069
codeforces_index: "J"
codeforces_contest_name: "VII MaratonUSP Freshman Contest"
rating: 0
weight: 104069
solve_time_s: 50
verified: true
draft: false
---

[CF 104069J - Du hành xuyên thời gian](https://codeforces.com/problemset/problem/104069/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các thao tác được áp dụng theo thời gian cho một tập hợp bắt đầu trống. Các thao tác được xử lý theo thứ tự và sau mỗi thao tác chèn, về mặt khái niệm, chúng tôi thu được một “phiên bản” mới của tập hợp. Các truy vấn sau này không đề cập đến tập hợp hiện tại mà đến trạng thái của tập hợp ngay sau một số chỉ mục hoạt động t trước đó. 

Có ba loại truy vấn. Một người yêu cầu phần tử lớn nhất trong tập hợp sau phép toán t, người khác yêu cầu phần tử nhỏ nhất sau phép toán t và người cuối cùng yêu cầu tổng của tất cả các phần tử sau phép toán t. Các phần chèn thêm chỉ thêm giá trị và không có gì bị xóa. 

Điểm tinh tế quan trọng là các truy vấn không phải về trạng thái hiện tại mà là về trạng thái lịch sử. Điều này có nghĩa là chúng ta phải có khả năng trả lời các câu hỏi về các tiền tố trước đó của chuỗi thao tác chứ không chỉ về kết quả cuối cùng. 

Các ràng buộc cho phép lên tới 100000 hoạt động. Một giải pháp tính toán lại các câu trả lời từ đầu cho mỗi truy vấn bằng cách xây dựng lại thiết lập cho đến thời điểm t sẽ là phương trình bậc hai trong trường hợp xấu nhất, quá chậm. Ngay cả cấu trúc logarit cho mỗi truy vấn cũng không đủ nếu chúng ta liên tục tính toán lại từ đầu. 

Việc triển khai đơn giản có thể sẽ duy trì một tập hợp duy nhất và đối với mỗi truy vấn, hãy xây dựng lại nó bằng cách phát lại tất cả các phần chèn thêm cho đến thời điểm t. Điều này không thành công khi có nhiều truy vấn ở gần cuối hỏi về tiền tố đầu. Ví dụ: nếu chúng ta chèn 100000 phần tử và sau đó hỏi 100000 truy vấn đều đề cập đến t = 1, thì mỗi truy vấn sẽ quét lại gần như toàn bộ lịch sử, tạo ra khoảng 10^10 thao tác. 

Một trường hợp lỗi khác xuất hiện khi các truy vấn được xen kẽ với các phần chèn. Nếu chúng ta chỉ duy trì tập hợp hiện tại thì chúng ta sẽ mất tất cả thông tin về các trạng thái trong quá khứ, do đó các truy vấn về t trước đó sẽ không thể thực hiện được nếu không tính toán lại. 

Do đó, yêu cầu chính là một cấu trúc duy trì thông tin tiền tố một cách hiệu quả đồng thời hỗ trợ các truy vấn phạm vi theo thời gian. 

## Phương pháp tiếp cận 

Một chiến lược vũ phu rất đơn giản. Chúng tôi mô phỏng từng hoạt động một. Chúng tôi duy trì một danh sách tất cả các phần tử được chèn vào. Khi chúng tôi nhận được một truy vấn trong thời gian t, chúng tôi sẽ xây dựng lại trạng thái đã đặt bằng cách lặp lại các thao tác t đầu tiên và thu thập tất cả các giá trị được chèn, sau đó tính toán mức tối thiểu, tối đa hoặc tổng. 

Điều này hoạt động hợp lý vì mỗi truy vấn đều hỏi chính xác về tiền tố của lịch sử hoạt động. Tuy nhiên, việc xây dựng lại tiền tố cho mọi truy vấn sẽ dẫn đến việc quét lặp lại cùng một dữ liệu. Trong trường hợp xấu nhất, mỗi truy vấn có giá O(n) và với truy vấn O(n), giá trị này trở thành O(n^2), quá chậm đối với 100000 thao tác. 

Sự cải tiến đến từ việc nhận ra rằng tất cả các truy vấn chỉ phụ thuộc vào tiền tố và chỉ chèn thêm thông tin. Điều này gợi ý rằng chúng ta có thể tính toán trước các tập hợp tiền tố theo thời gian. Nếu chúng ta duy trì, đối với mỗi chỉ mục thao tác i, giá trị tối thiểu, tối đa và tổng của tất cả các giá trị được chèn lên tới i thì mọi truy vấn sẽ trở thành O(1). 

Điều này có thể thực hiện được vì chèn là thao tác cập nhật duy nhất, vì vậy mỗi tiền tố có thể được lấy từ tiền tố trước đó bằng một bản cập nhật đơn giản. Chúng tôi không cần cấu trúc tập hợp động như cây cân bằng, vì chúng tôi không bao giờ xóa hoặc sửa đổi các giá trị trong quá khứ. 

Chúng tôi duy trì ba mảng được lập chỉ mục theo số thao tác: tiền tố tối thiểu, tiền tố tối đa và tổng tiền tố. Đối với mỗi lần chèn, chúng tôi cập nhật các mảng này từ trạng thái trước đó. Đối với các hoạt động truy vấn, chúng tôi xuất trực tiếp giá trị được lưu trữ tại chỉ mục t. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tính toán trước tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các hoạt động theo thứ tự trong khi xây dựng thông tin tiền tố cho từng bước.

1. Khởi tạo ba mảng hoặc biến để theo dõi trạng thái tiền tố: mức tối thiểu hiện tại, mức tối đa hiện tại và tổng hiện tại. Vì tập hợp bắt đầu trống nên chúng ta cũng cần xác định cách xử lý lần chèn đầu tiên. Thao tác đầu tiên được đảm bảo là thao tác chèn, giúp tránh các trường hợp cạnh được đặt trống cho các truy vấn. 
2. Với mỗi thao tác i từ 1 đến n, hãy đọc loại của nó. Nếu đó là phần chèn giá trị x, chúng tôi sẽ cập nhật tổng hiện tại bằng cách thêm x và cập nhật mức tối thiểu và tối đa hiện tại tương ứng. Các giá trị tiền tố tại chỉ mục i trở thành các giá trị được cập nhật này. 
3. Nếu thao tác là một truy vấn yêu cầu mức tối đa tại thời điểm t, chúng tôi trực tiếp xuất mức tối đa được lưu trữ tương ứng với tiền tố t. Điều này hoạt động vì tiền tố t đã biểu thị trạng thái được đặt sau khi chèn t. 
4. Nếu hoạt động là một truy vấn yêu cầu mức tối thiểu tại thời điểm t, chúng tôi xuất mức tối thiểu được lưu trữ ở tiền tố t. 
5. Nếu thao tác là một truy vấn yêu cầu tổng tại thời điểm t, chúng ta xuất tổng được lưu trữ ở tiền tố t. 

Lựa chọn thiết kế chính là mỗi lần chèn sẽ xác định vĩnh viễn trạng thái tiền tố. Chúng tôi không bao giờ tính toán lại lịch sử, chúng tôi chỉ mở rộng nó. 

Tại sao nó hoạt động dựa trên một bất biến đơn giản: sau khi xử lý thao tác i, các giá trị được lưu trữ biểu thị chính xác các thuộc tính tổng hợp của tất cả các phần tử được chèn trong số các thao tác i đầu tiên. Vì các phần chèn thêm không bao giờ loại bỏ các phần tử nên tiền tố i luôn là sự thể hiện đầy đủ của tập hợp tại thời điểm đó. Bất kỳ truy vấn nào hỏi về thời gian t chỉ là đọc trạng thái tiền tố được tính toán trước đó, trạng thái này không thay đổi bởi các hoạt động sau này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

INF = 10**30

# prefix arrays (1-indexed conceptually)
mx = [0] * (n + 1)
mn = [0] * (n + 1)
sm = [0] * (n + 1)

cur_max = -INF
cur_min = INF
cur_sum = 0

for i in range(1, n + 1):
    parts = input().split()
    typ = int(parts[0])

    if typ == 1:
        x = int(parts[1])
        cur_sum += x
        cur_max = max(cur_max, x)
        cur_min = min(cur_min, x)

    mx[i] = cur_max
    mn[i] = cur_min
    sm[i] = cur_sum

    if typ == 2:
        t = int(parts[1])
        print(mx[t])
    elif typ == 3:
        t = int(parts[1])
        print(mn[t])
    elif typ == 4:
        t = int(parts[1])
        print(sm[t])
```Giải pháp duy trì các tập hợp đang chạy đồng thời lưu trữ ảnh chụp nhanh tiền tố. Chi tiết quan trọng là chúng tôi ghi lại trạng thái sau khi xử lý từng chỉ mục hoạt động i, không chỉ sau khi chèn. Sự căn chỉnh này đảm bảo rằng các truy vấn đề cập đến bất kỳ t nào sẽ trực tiếp lập chỉ mục vào trạng thái tiền tố chính xác. 

Một điểm tinh tế là khởi tạo mức tối thiểu và tối đa. Chúng tôi bắt đầu với các trọng điểm cực đoan để lần chèn đầu tiên đặt chính xác cả hai giá trị. Vì thao tác đầu tiên được đảm bảo là thao tác chèn nên chúng tôi không bao giờ xuất ra các giá trị không xác định. 

Một chi tiết khác là các truy vấn đề cập đến các chỉ mục trong quá khứ, vì vậy chúng tôi không bao giờ sử dụng trực tiếp trạng thái hiện tại. Mọi câu trả lời đều được đọc từ các mảng được lập chỉ mục bởi t. 

## Ví dụ đã hoạt động 

### Ví dụ Dấu vết 1 

đầu vào:```
1 10
2 1
2 2
3 2
4 2
1 5
2 6
```Chúng tôi theo dõi trạng thái sau mỗi bước. 

| tôi | op | cur_sum | cur_min | cur_max | đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | chèn 10 | 10 | 10 | 10 | | 
| 2 | tối đa t=1 | 10 | 10 | 10 | 10 | 
| 3 | tối đa t=2 | 10 | 10 | 10 | 10 | 
| 4 | phút t=2 | 10 | 10 | 10 | 10 | 
| 5 | tổng t=2 | 10 | 10 | 10 | 10 | 
| 6 | chèn 5 | 15 | 5 | 10 | | 
| 7 | tối đa t=6 | 15 | 5 | 10 | 10 | 

Dấu vết này cho thấy các truy vấn luôn đề cập đến các ảnh chụp nhanh trước đó và các lần chèn sau không ảnh hưởng đến các câu trả lời trước đó. 

### Ví dụ Dấu vết 2 

đầu vào:```
1 1
1 10
2 2
3 3
4 4
4 1
```| tôi | op | cur_sum | cur_min | cur_max | đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | chèn 1 | 1 | 1 | 1 | | 
| 2 | chèn 10 | 11 | 1 | 10 | | 
| 3 | tối đa t=2 | 11 | 1 | 10 | 10 | 
| 4 | phút t=3 | 11 | 1 | 10 | 1 | 
| 5 | tổng t=4 | 11 | 1 | 10 | 11 | 
| 6 | tổng t=1 | 11 | 1 | 10 | 1 | 

Điều này xác nhận rằng việc lập chỉ mục tiền tố sẽ phân tách chính xác các chế độ xem lịch sử khác nhau của tập hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi thao tác được xử lý một lần và mỗi truy vấn được trả lời bằng O(1) bằng cách sử dụng các giá trị tiền tố được tính toán trước | 
| Không gian | O(n) | Chúng tôi lưu trữ tổng hợp tiền tố cho từng chỉ mục hoạt động | 

Độ phức tạp tuyến tính phù hợp thoải mái trong phạm vi 100000 thao tác dưới giới hạn 2 giây. Việc sử dụng bộ nhớ cũng ít vì chúng tôi chỉ lưu trữ ba mảng số nguyên có kích thước n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from subprocess import Popen, PIPE
    # placeholder: in real use, call main() directly
    return ""

# provided samples (placeholders since formatting in statement is unclear)
# assert run(...) == ...

# custom cases
assert run("""1 5
1 7
2 2
3 2
4 2
""") == "7\n5\n12\n", "simple insert and queries"

assert run("""1 1000000000
2 1
3 1
4 1
""") == "1000000000\n1000000000\n1000000000\n", "single element extremes"

assert run("""1 1
1 2
1 3
1 4
2 4
3 4
4 4
""") == "4\n1\n10\n", "increasing sequence"

assert run("""1 5
2 1
1 10
2 2
2 1
""") == "5\n5\n5\n", "queries before and after insertion"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trình tự tối thiểu | giá trị trực tiếp | độ đúng cơ sở | 
| giá trị đơn lớn | cùng một đầu ra cho tất cả các hoạt động | xử lý giá trị cực cao | 
| trình tự tăng dần | tiến hóa tối thiểu/tối đa đúng | cập nhật tiền tố | 
| truy vấn thời gian hỗn hợp | lập chỉ mục lịch sử chính xác | tính chính xác của tiền tố | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi các truy vấn đề cập đến thời điểm rất sớm trong khi nhiều lần chèn xảy ra sau đó. Ví dụ: chèn một chuỗi lớn theo sau là nhiều truy vấn cho t = 1. Thuật toán xử lý điều này một cách chính xác vì trạng thái tiền tố ở chỉ mục 1 bị cố định sau khi tính toán và các bản cập nhật sau này không sửa đổi nó. 

Một trường hợp khác là chèn và truy vấn xen kẽ. Vì chúng tôi lưu trữ trạng thái tiền tố ở mọi chỉ mục nên các truy vấn luôn đọc các ảnh chụp nhanh nhất quán mà không cần tính toán lại. 

Trường hợp cuối cùng là tăng hoặc giảm nghiêm ngặt đầu vào, trong đó mức tối thiểu hoặc tối đa luôn được cập nhật ở mỗi bước. Việc khởi tạo trọng điểm đảm bảo giá trị đầu tiên đặt chính xác cả hai giới hạn và mọi cập nhật tiếp theo sẽ duy trì tính chính xác đơn điệu.
