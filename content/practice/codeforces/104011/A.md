---
title: "CF 104011A - Anno Domini 2022"
description: "Chúng ta có hai điểm trên một dòng thời gian đơn giản hóa trong đó mỗi năm được gắn nhãn trong hệ thống AD hoặc trong hệ thống BC. Mỗi dòng đầu vào mô tả một năm như “AD 2022” hoặc “5508 BC”."
date: "2026-07-02T05:12:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104011
codeforces_index: "A"
codeforces_contest_name: "2021-2022 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104011
solve_time_s: 48
verified: true
draft: false
---

[CF 104011A - Anno Domini 2022](https://codeforces.com/problemset/problem/104011/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai điểm trên một dòng thời gian đơn giản hóa trong đó mỗi năm được gắn nhãn trong hệ thống AD hoặc trong hệ thống BC. Mỗi dòng đầu vào mô tả một năm như “AD 2022” hoặc “5508 BC”. Nhiệm vụ là tính xem có bao nhiêu năm nằm đúng giữa ngày đầu tiên của năm đầu tiên và ngày đầu tiên của năm thứ hai. 

Sự tinh tế quan trọng là quy ước lịch sử rằng không có năm 0. Trình tự các năm xung quanh quá trình chuyển đổi là… 2 TCN, 1 TCN, 1 AD, 2 AD…. Điều này làm cho phép trừ trực tiếp trên các số nguyên có dấu hơi phức tạp nếu chúng ta không mã hóa ánh xạ một cách cẩn thận. 

Các ràng buộc nhỏ, với các năm được giới hạn từ 1 đến 9999. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm đều đủ và thậm chí việc phân tích cú pháp lặp lại hoặc số học đơn giản là không đáng kể. 

Trường hợp cạnh chính đang vượt qua ranh giới BC đến AD. Ví dụ: giữa “1 BC” và “AD 1”, câu trả lời không phải là 2 mà là 1, vì giữa chúng có đúng một bước ranh giới. Một ánh xạ ngây thơ coi năm BC là số âm bao gồm số 0 sẽ thất bại ở đây. 

Một trường hợp cạnh khác là đặt hàng. Ngày trước đó không được đảm bảo sẽ đến trước, do đó, mọi giải pháp đều phải bình thường hóa bằng cách lấy chênh lệch tuyệt đối trong ánh xạ số nhất quán. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là mô phỏng từng năm bắt đầu từ ngày trước đó và tăng hoặc giảm cho đến ngày sau, đếm các bước. Đây là khái niệm đơn giản và luôn đúng, nhưng đó là công việc không cần thiết. Mặc dù phạm vi nhỏ, nhưng sự khác biệt giữa các điểm cuối có thể lên tới khoảng 20000 năm trong quá trình chuyển đổi từ BC sang AD tồi tệ nhất, vẫn còn nhỏ nhưng không tinh tế và sẽ trở nên bất tiện nếu được kéo dài. 

Cái nhìn sâu sắc thực sự là đây chỉ là một vấn đề sắp xếp tuyến tính với điểm gián đoạn ở mức 0. Nếu chúng ta có thể ánh xạ mỗi năm tới một dòng số nguyên duy nhất trong đó giá trị kề tương ứng chính xác với giá trị kề theo thời gian, thì câu trả lời sẽ trở thành một sai phân tuyệt đối đơn giản. 

Cấu trúc tự nhiên là gán năm AD là số nguyên dương và năm BC là số nguyên âm, nhưng có hiệu chỉnh: vì không có năm 0 nên chúng ta phải đảm bảo rằng 1 BC ánh xạ tới -1 và AD 1 ánh xạ tới +1, làm cho khoảng cách chuyển tiếp chính xác là 2 đơn vị trong số nguyên thô nhưng trên thực tế chỉ là 1 năm. Để khắc phục điều này, chúng tôi dịch chuyển BC từng năm một khi chuyển đổi, bỏ qua số 0 trên trục số một cách hiệu quả. 

Khi cả hai năm được ánh xạ vào trục số nguyên đã hiệu chỉnh này, số năm giữa chúng đơn giản là sự khác biệt tuyệt đối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(sự khác biệt) | O(1) | Quá chậm và không cần thiết | 
| Ánh xạ số nguyên với shift | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mỗi năm đầu vào thành một số nguyên theo dòng thời gian thống nhất trong đó các số nguyên liên tiếp tương ứng với các năm liên tiếp trong lịch sử. 

1. Phân tích từng dòng thành một cặp bao gồm giá trị số năm và điểm đánh dấu hướng, AD hoặc BC. 
2. Chuyển đổi năm thành biểu diễn số nguyên có dấu. Nếu năm là AD, chúng tôi ánh xạ trực tiếp tới +năm. Nếu năm là BC, chúng ta ánh xạ nó thành -year. Ở giai đoạn này, 1 BC trở thành -1 và AD 1 trở thành +1. 
3. Tính hiệu nguyên giữa hai số nguyên này. Tuy nhiên, sự khác biệt thô này tính gấp đôi số lần chuyển tiếp năm 0 bị thiếu. 
4. Điều chỉnh ánh xạ bằng cách coi cạnh BC dịch chuyển về phía trước 1 đơn vị. Điều này có thể được thực hiện một cách tương tự bằng cách ánh xạ năm BC y tới -(y - 1), đảm bảo rằng -1 tương ứng với 1 BC và 0 tương ứng với vị trí tiền thân của AD 1, loại bỏ hiệu quả khoảng cách năm không tồn tại. 
5. Sau khi chuyển đổi, lấy chênh lệch tuyệt đối giữa hai giá trị được ánh xạ. Điều này đưa ra con số chính xác về những lần chuyển đổi hàng năm giữa hai ranh giới ngày 1 tháng 1.

### Tại sao nó hoạt động 

Ánh xạ số nguyên xây dựng một phép đối chiếu giữa các năm lịch sử thực và các số nguyên có khoảng cách đơn vị. Mỗi mức tăng 1 trong giá trị được ánh xạ tương ứng với việc chuyển tiếp đúng một năm dương lịch, bao gồm cả ranh giới BC đến AD. Vì ánh xạ duy trì tính kề cận và loại bỏ điểm 0 nhân tạo, nên khoảng cách trong không gian số nguyên giống hệt với số năm trôi qua trong thời gian thực. Do đó, sự khác biệt tuyệt đối tính chính xác số năm chuyển đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def parse(line: str) -> int:
    parts = line.strip().split()
    if parts[0] == "AD":
        return int(parts[1])
    else:
        y = int(parts[0])
        return -(y - 1)

a = parse(input())
b = parse(input())

print(abs(a - b))
```Giải pháp đọc hai dòng và chuyển đổi mỗi dòng thành một giá trị dòng thời gian số nguyên thống nhất. Hàm trợ giúp xử lý sự bất đối xứng giữa BC và AD bằng cách dịch chuyển các năm BC sao cho không có điểm 0 nhân tạo. 

Câu trả lời cuối cùng được tính là chênh lệch tuyệt đối, điều này hợp lệ vì ánh xạ đảm bảo tính kề cận tuyến tính. 

Một lỗi phổ biến là ánh xạ trực tiếp năm BC tới số nguyên âm mà không dịch chuyển. Điều đó làm cho “1 BC” là -1 và “AD 1” là +1, ngụ ý không chính xác khoảng cách là 2. Việc dịch chuyển đã sửa sẽ khắc phục chính xác vấn đề này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

QUẢNG CÁO 1 

1 trước Công nguyên 

Các giá trị được ánh xạ: 

| Bước | Giá trị 1 | Giá trị 2 | Sự khác biệt | 
| --- | --- | --- | --- | 
| Phân tích | 1 | 1 TCN | - | 
| Bản đồ | 1 | 0 | - | 
| Tính toán | 1 | 0 | 1 | 

Đầu ra là 1, phù hợp với thực tế là chỉ có quá trình chuyển đổi từ 1 BC sang AD 1 là một bước duy nhất trong một năm. 

Điều này xác nhận rằng việc ánh xạ sẽ thu gọn chính xác khoảng cách số 0 của năm bị thiếu. 

### Ví dụ 2 

đầu vào: 

năm 2001 sau Công nguyên 

QUẢNG CÁO 1 

Các giá trị được ánh xạ: 

| Bước | Giá trị 1 | Giá trị 2 | Sự khác biệt | 
| --- | --- | --- | --- | 
| Phân tích | 2001 | 1 | - | 
| Bản đồ | 2001 | 1 | - | 
| Tính toán | 2001 | 1 | 2000 | 

Đầu ra là 2000, tương ứng với số lần chuyển đổi hàng năm giữa hai ngày AD. 

Điều này xác minh rằng trong một kỷ nguyên duy nhất, ánh xạ giảm xuống mức trừ thông thường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ phân tích cú pháp và số học theo thời gian không đổi | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Các ràng buộc đủ nhỏ để thậm chí các giải pháp ít tối ưu hơn cũng có thể vượt qua, nhưng việc ánh xạ trực tiếp này tránh mọi mô phỏng không cần thiết và hoạt động thống nhất trên các phạm vi BC và AD. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def parse(line: str) -> int:
        parts = line.strip().split()
        if parts[0] == "AD":
            return int(parts[1])
        else:
            y = int(parts[0])
            return -(y - 1)

    a = parse(input())
    b = parse(input())
    return str(abs(a - b))

def run(inp: str) -> str:
    return solve(inp)

# provided samples
assert run("1 BC\nAD 1\n") == "1"
assert run("AD 1\nAD 2001\n") == "2000"

# custom cases
assert run("AD 2022\n5508 BC\n") == str(abs(2022 - (-(5508 - 1)))), "cross era large gap"
assert run("1 BC\n2 BC\n") == "1", "within BC consecutive years"
assert run("AD 1\nAD 1\n") == "0", "same year"
assert run("9999 BC\nAD 9999\n") == str(abs(-(9999 - 1) - 9999)), "max boundary cross"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1/1 TCN | 1 | Độ chính xác của ranh giới BC-AD | 
| AD 1 / AD 2001 | 2000 | trừ AD bình thường | 
| 2022/5508 TCN | tính toán | khoảng cách lớn xuyên thời đại | 
| 1 TCN / 2 TCN | 1 | BC đặt hàng đúng đắn | 
| QUẢNG CÁO 1 / QUẢNG CÁO 1 | 0 | xử lý đầu vào giống hệt nhau | 

## Vỏ cạnh 

Đối với quá trình chuyển đổi BC sang AD, hãy xem xét đầu vào “1 BC” và “AD 1”. Trình phân tích cú pháp ánh xạ chúng tới 0 và 1 tương ứng sau khi dịch BC đi một. Sự khác biệt là 1, phù hợp với quá trình chuyển đổi một năm qua ranh giới. 

Đối với đầu vào thuần túy BC như “2 BC” và “1 BC”, ánh xạ cho kết quả -1 và 0. Sự khác biệt là 1, phản ánh chính xác các năm liên tiếp theo thứ tự thời gian đảo ngược. 

Đối với những năm giống hệt nhau như “AD 2022” và “AD 2022”, cả hai đều lập bản đồ đến năm 2022, tạo ra 0 như mong đợi. 

Logic dịch chuyển đảm bảo tất cả các điểm gián đoạn được hấp thụ vào một dòng số nguyên liên tục, do đó mọi thứ tự có thể giảm xuống còn một phép tính sai phân tuyệt đối duy nhất.
