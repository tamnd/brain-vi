---
title: "CF 106258A - Điều dễ dàng"
description: "Nhiệm vụ cực kỳ nhỏ và trực tiếp: chúng tôi nhận được một chuỗi câu trả lời từ nhiều người, trong đó mỗi câu trả lời là “điều này dễ” hoặc “điều này khó”. Mục đích là để quyết định xem vấn đề có còn được coi là dễ dàng về tổng thể hay không. Quy tắc rất đơn giản."
date: "2026-06-19T01:12:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106258
codeforces_index: "A"
codeforces_contest_name: "Small Imprecision Contest"
rating: 0
weight: 106258
solve_time_s: 220
verified: true
draft: false
---

[CF 106258A - The Easy One](https://codeforces.com/problemset/problem/106258/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ cực kỳ nhỏ và trực tiếp: chúng tôi nhận được một chuỗi câu trả lời từ nhiều người, trong đó mỗi câu trả lời là “điều này dễ” hoặc “điều này khó”. Mục đích là để quyết định xem vấn đề có còn được coi là dễ dàng về tổng thể hay không. 

Quy tắc rất đơn giản. Nếu chỉ có một người cho rằng vấn đề là khó thì phán quyết cuối cùng sẽ trở thành “khó”. Chỉ khi mọi người đồng ý rằng nó dễ thì chúng ta mới chấp nhận nó là dễ. 

Vì vậy, đầu vào chỉ là một danh sách các ý kiến ​​nhị phân, trong đó một giá trị đại diện cho “dễ” và giá trị còn lại đại diện cho “khó”. Đầu ra là một từ duy nhất tóm tắt xem có tồn tại bất kỳ ý kiến ​​“cứng rắn” nào hay không. 

Hạn chế rất nhỏ, tối đa 100 ý kiến. Điều đó ngay lập tức loại trừ mọi lo ngại về hiệu suất. Ngay cả việc quét bậc hai hoặc lồng nhau cũng được, nhưng cấu trúc cho thấy chúng ta chỉ cần một lần quét duy nhất. 

Các trường hợp chính chủ yếu là về đầu vào tối thiểu và đầu vào đồng nhất. Nếu chỉ có một người và họ nói dễ thì câu trả lời phải dễ. Nếu chính người đó nói khó, câu trả lời sẽ thay đổi ngay lập tức. Một trường hợp tinh vi khác là khi ý kiến ​​cứng rắn xuất hiện ở phần cuối hoặc phần đầu, điều này không ảnh hưởng đến tính đúng đắn nhưng có thể làm lộ ra logic chấm dứt sớm không chính xác nếu ai đó thực hiện nó một cách bất cẩn. 

## Phương pháp tiếp cận 

Cách diễn giải thô bạo sẽ coi đầu vào là một tập hợp các giá trị và liên tục kiểm tra các tập hợp con hoặc mô phỏng một quy trình quyết định. Ví dụ, người ta có thể tưởng tượng việc kiểm tra mọi tiền tố và tính toán lại xem tiền tố đó có “sạch” các ý kiến ​​cứng rắn hay không. Điều này đúng nhưng lặp đi lặp lại một cách không cần thiết, vì mỗi lần kiểm tra sẽ quét lại các phần tử giống nhau. Trong trường hợp xấu nhất, nó trở thành phương trình bậc hai về số người, khoảng$n^2$, điều này vẫn còn tầm thường đối với$n \le 100$nhưng đã chỉ ra công việc lãng phí. 

Quan sát quan trọng là quyết định chỉ phụ thuộc vào sự tồn tại. Chúng tôi không quan tâm có bao nhiêu người nghĩ nó khó, cũng như họ xuất hiện ở đâu. Chúng ta chỉ cần biết liệu có ít nhất một số “1” tồn tại trong danh sách hay không. Điều đó làm giảm toàn bộ vấn đề chỉ còn một lần quét bằng cờ boolean. 

Một khi điều này được nhìn thấy, giải pháp sẽ trở thành một cuộc kiểm tra trực tuyến: ngay khi một ý kiến ​​cứng rắn xuất hiện, câu trả lời sẽ được xác định. Không cần phải xử lý thêm các yếu tố cho chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra lại tiền tố) | O(n²) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Quét một lượt | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số ý kiến. Điều này cho chúng tôi biết chúng tôi sẽ quét bao nhiêu giá trị nhưng không ảnh hưởng đến logic ngoài giới hạn vòng lặp. 
2. Khởi tạo cờ`hard_found`như sai. Biến này thể hiện liệu chúng ta đã gặp phải bất kỳ ý kiến ​​nào đánh giá vấn đề là khó hay chưa. 
3. Lặp lại từng câu trả lời trong danh sách. Đối với mỗi giá trị, hãy kiểm tra xem nó có bằng 1 hay không, đại diện cho phiếu bầu “cứng rắn”. 
4. Nếu tìm thấy phiếu bầu cứng, hãy đặt`hard_found`thành sự thật. Tại thời điểm này, câu trả lời cuối cùng đã được xác định một cách hợp lý, ngay cả khi chúng tôi tiếp tục tìm kiếm để đơn giản hóa việc triển khai. 
5. Sau khi xử lý tất cả các giá trị, xuất ra “HARD” nếu`hard_found`là đúng, nếu không thì xuất ra “EASY”. 

Lý do đằng sau việc phát hiện sớm là vấn đề thực sự là việc giảm OR trên tất cả các đầu vào. Khi một giá trị thực được nhìn thấy, OR toàn cục đã đúng. 

### Tại sao nó hoạt động 

Quy tắc quyết định tương đương với việc tính toán logic HOẶC trên tất cả các giá trị đầu vào, trong đó 1 đóng góp đúng và 0 đóng góp sai. Câu trả lời cuối cùng chỉ phụ thuộc vào việc OR này có đúng hay không. Vì OR là lũy đẳng và không phụ thuộc vào thứ tự nên việc quét tuần tự và theo dõi xem có bất kỳ giá trị thực nào xuất hiện hay không sẽ duy trì tính chính xác. Không cần sắp xếp lại hoặc đếm và không có giá trị nào sau này có thể ghi đè lên quan điểm cứng rắn đã thấy trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
arr = list(map(int, input().split()))

hard_found = False

for x in arr:
    if x == 1:
        hard_found = True

print("HARD" if hard_found else "EASY")
```Giải pháp đọc tất cả các giá trị trong một lần và quét chúng một lần. Trạng thái duy nhất chúng tôi duy trì là cờ boolean. Không cần phải thoát sớm, mặc dù nó có thể được thêm vào mà không làm thay đổi tính chính xác. 

Một lỗi triển khai phổ biến là cố gắng quá thông minh trong việc đếm hoặc phân đoạn logic. Điều đó giới thiệu sự phức tạp không cần thiết. Một vấn đề khác là quên rằng đầu vào là các số nguyên được phân tách bằng dấu cách trên một dòng, điều này có thể dẫn đến việc đọc một phần nếu xử lý không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
0 0 1
```Chúng tôi quét mảng từng bước. 

| Chỉ mục | Giá trị | cứng_tìm thấy | 
| --- | --- | --- | 
| 1 | 0 | sai | 
| 2 | 0 | sai | 
| 3 | 1 | đúng | 

Sau khi quét, tồn tại ít nhất một ý kiến ​​cứng rắn, do đó kết quả là:```
HARD
```Dấu vết này cho thấy chỉ một lần xuất hiện muộn cũng đủ để lật tẩy quyết định cuối cùng. 

### Ví dụ 2 

đầu vào:```
1
0
```| Chỉ mục | Giá trị | cứng_tìm thấy | 
| --- | --- | --- | 
| 1 | 0 | sai | 

Không có giá trị cứng nào được nhìn thấy, vì vậy kết quả là:```
EASY
```Điều này xác nhận hành vi trên kích thước đầu vào tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ý kiến ​​được kiểm tra chính xác một lần trong lần quét tuyến tính | 
| Không gian | O(1) | Chỉ có một cờ boolean duy nhất được lưu trữ | 

Với$n \le 100$, điều này thấp hơn nhiều so với mọi giới hạn thực tế. Ngay cả khi các ràng buộc lớn hơn, quá trình quét tuyến tính tương tự vẫn tối ưu vì mỗi đầu vào phải được đọc ít nhất một lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    input = sys.stdin.readline

    n = int(input())
    arr = list(map(int, input().split()))

    hard_found = False
    for x in arr:
        if x == 1:
            hard_found = True

    return "HARD" if hard_found else "EASY"

# provided samples
assert run("3\n0 0 1\n") == "HARD"
assert run("1\n0\n") == "EASY"

# custom cases
assert run("5\n0 0 0 0 0\n") == "EASY", "all easy"
assert run("4\n1 1 1 1\n") == "HARD", "all hard"
assert run("6\n0 1 0 0 0 0\n") == "HARD", "single early hard"
assert run("2\n0 1\n") == "HARD", "minimal mix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | DỄ DÀNG | không có phiếu cứng | 
| tất cả những cái | CỨNG | hộp cứng thống nhất | 
| sớm khó khăn | CỨNG | chấm dứt sớm đúng đắn | 
| khó khăn cuối cùng | CỨNG | phát hiện muộn đúng đắn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi chỉ có một người tham gia. Thuật toán vẫn hoạt động vì vòng lặp chạy đúng một lần và phản ánh trực tiếp ý kiến ​​​​duy nhất đó. 

Một trường hợp khác là khi ý kiến ​​cứng rắn xuất hiện ở phút cuối. Việc triển khai đơn giản giả định không chính xác việc chấm dứt sớm hoặc đọc sai thông tin đầu vào có thể bỏ lỡ nó, nhưng quá trình quét tuyến tính sẽ cập nhật chính xác cờ khi nó xuất hiện. 

Trường hợp cuối cùng là khi tất cả các giá trị đều dễ dàng. Ở đây, cờ vẫn sai xuyên suốt và đầu ra chính xác vẫn là “DỄ DÀNG”, xác nhận rằng việc thiếu bằng chứng được coi là không có độ cứng chứ không phải là mơ hồ.
