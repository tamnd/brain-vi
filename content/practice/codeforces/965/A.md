---
problem: 965A
contest_id: 965
problem_index: A
name: "Paper Airplanes"
contest_name: "Codeforces Round 476 (Div. 2) [Thanks, Telegram!]"
rating: 800
tags: ["math"]
answer: passed_samples
verified: true
solve_time_s: 65
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a3281e5-4e50-83ec-8a25-9981e3e4dcdf
---

#CF 965A - Máy Bay Giấy 

**Đánh giá:** 800 
**Thẻ:** toán 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a3281e5-4e50-83ec-8a25-9981e3e4dcdf 

--- 

##Giải pháp 

## Hiểu vấn đề 

Một nhóm người muốn sản xuất những chiếc máy bay giấy giống hệt nhau, nhưng cách duy nhất để chế tạo những chiếc máy bay là gián tiếp thông qua những tờ giấy. Mỗi tờ giấy có thể được gấp lại thành một số lượng máy bay cố định và giấy chỉ được bán theo gói, mỗi gói chứa một số tờ cố định. 

Yêu cầu có cấu trúc đơn giản nhưng có nhiều ràng buộc. Mỗi người phải có khả năng độc lập chế tạo một số lượng máy bay cố định. Vì nhiều máy bay xuất phát từ một tờ giấy duy nhất nên trước tiên, chúng tôi chuyển đổi yêu cầu về máy bay thành yêu cầu tờ giấy cho mỗi người, sau đó chia tỷ lệ theo số người và cuối cùng chuyển đổi các tờ giấy thành gói vì không thể mua gói lẻ. 

Số lượng quan trọng là tổng cộng cần bao nhiêu tờ. Nếu mỗi tờ tạo ra`s`máy bay thì mỗi người cần`ceil(n / s)`trang tính, vì các trang một phần không tồn tại. Sang`k`mọi người, tổng số trang yêu cầu sẽ trở thành`k * ceil(n / s)`. Khi đã biết số tờ, chúng tôi chuyển nó thành các gói có kích thước`p`sử dụng một bộ phận trần khác. 

Các ràng buộc đủ nhỏ để bất kỳ giải pháp số học theo thời gian không đổi nào cũng đủ. Tất cả các biến có nhiều nhất là 10^4, do đó, ngay cả phép nhân trực tiếp cũng chỉ đạt 10^8, nằm trong phạm vi số nguyên thoải mái đối với Python và không yêu cầu tối ưu hóa ngoài tính toán O(1). 

Nơi chính xảy ra lỗi là trong việc xử lý làm tròn một cách chính xác. Ví dụ, nếu một người cần 5 chiếc máy bay và mỗi tấm tạo ra 2 chiếc máy bay thì 2 tấm chỉ tạo ra 4 chiếc máy bay, điều này là không đủ. Một phép chia số nguyên đơn giản sẽ gán không chính xác 2 trang tính thay vì 3. Vấn đề tương tự lại xuất hiện khi chuyển đổi các trang tính thành gói. 

Một sai lầm tinh vi khác là áp dụng cách chia trần quá muộn hoặc quá sớm. Ví dụ: làm tròn sau khi nhân với`k`thay vì mỗi người vẫn hoạt động ở đây do tính tuyến tính, nhưng làm tròn sai giai đoạn khi thực hiện bất cẩn thường dẫn đến sai sót từng cái một. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là mô phỏng việc mua từng gói một, phân phát các tờ giấy và kiểm tra xem liệu tất cả mọi người có thể sản xuất đủ máy bay hay không. Sau mỗi gói bổ sung, chúng tôi sẽ tính toán lại số lượng máy bay có thể được sản xuất và xác minh yêu cầu. Mặc dù đơn giản về mặt khái niệm, cách tiếp cận này không hiệu quả vì số lượng gói có thể tăng lên lớn và mỗi lần kiểm tra yêu cầu tính toán lại tổng sản lượng trên tất cả các trang, dẫn đến đánh giá O(k) lặp đi lặp lại cho mỗi số gói ứng viên. 

Tuy nhiên, cấu trúc của vấn đề loại bỏ mọi nhu cầu mô phỏng. Mỗi gói đóng góp một số tờ cố định, mỗi gói đóng góp một số lượng máy bay cố định và mỗi người có một yêu cầu giống nhau. Điều này biến vấn đề thành một quy trình chuyển đổi xác định: máy bay của mỗi người thành tờ cho mỗi người, sau đó thành tổng số tờ, sau đó thành gói. 

Quan sát quan trọng là tất cả các phép biến đổi đều tuyến tính ngoại trừ việc làm tròn tại các ranh giới phân chia. Khi chúng tôi nhận ra rằng chúng tôi chỉ cần đáp ứng số lượng tờ tối thiểu toàn cầu, vấn đề sẽ giảm xuống việc tính toán một mức trần duy nhất hai lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(câu trả lời · k) | O(1) | Quá chậm | 
| Toán trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính xem một người cần làm bao nhiêu tờ giấy`n`máy bay. Vì mỗi tờ làm`s`máy bay, chúng tôi tính toán`need_per_person = ceil(n / s)`. Điều này đảm bảo không có người nào bị thiếu hụt ngay cả khi máy bay không chia đều cho mỗi tờ giấy. 
2. Chuyển yêu cầu này của mỗi người thành tổng yêu cầu bằng cách nhân với số người:`total_sheets = k * need_per_person`. Sự tổng hợp này hoạt động vì yêu cầu của mỗi người là độc lập và giống hệt nhau. 
3. Chuyển tờ giấy thành gói. Mỗi gói chứa`p`tờ, vậy số gói là`packs = ceil(total_sheets / p)`. Điều này đảm bảo chúng tôi không bao giờ mua gói một phần. 
4. In số gói đã tính. 

Sự tinh tế chính là đảm bảo rằng cả hai vách ngăn đều là vách ngăn trần chứ không phải vách ngăn sàn. Một bước làm tròn bị bỏ lỡ sẽ đánh giá thấp các yêu cầu và tạo ra các giải pháp không hợp lệ. 

### Tại sao nó hoạt động 

Thuật toán hoạt động vì mọi ràng buộc đều đơn điệu và tuyến tính: nhiều tấm hơn không bao giờ làm giảm khả năng sản xuất máy bay và mỗi phép biến đổi đều đảm bảo tính khả thi. Sự gián đoạn duy nhất đến từ tính không thể phân chia của các tờ trên mỗi máy bay và các gói trên mỗi tờ, và cả hai đều được xử lý chính xác thông qua việc phân chia trần. Sau khi tính cả hai điểm làm tròn, số được tính là giá trị tối thiểu thỏa mãn tất cả các ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

k, n, s, p = map(int, input().split())

need_per_person = (n + s - 1) // s
total_sheets = k * need_per_person
packs = (total_sheets + p - 1) // p

print(packs)
```Dòng đầu tiên tính toán mức trần cho máy bay trên mỗi tờ giấy. Cái thứ hai tổng hợp trên tất cả mọi người. Dòng cuối cùng chuyển các tờ giấy thành các gói bằng cách sử dụng cùng một mẫu trần. biểu hiện`(x + d - 1) // d`là một thủ thuật số nguyên tiêu chuẩn để chia trần trong Python, tránh số học dấu phẩy động và duy trì tính chính xác cho tất cả các phạm vi số nguyên trong các ràng buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3 2 3
```Ở đây mỗi tờ giấy làm được 2 chiếc máy bay nên mỗi người cần`ceil(3 / 2) = 2`tờ. 

| Bước | Giá trị | 
| --- | --- | 
| n | 3 | 
| s | 2 | 
| tờ mỗi người | 2 | 
| k | 5 | 
| tổng số tờ | 10 | 
| p | 3 | 
| gói | 4 | 

Mỗi gói có 3 tờ nên cần 10 tờ`ceil(10 / 3) = 4`gói. Điều này khẳng định sự cần thiết của việc làm tròn ở cả hai giai đoạn. 

### Ví dụ 2 

đầu vào:```
1 10 3 5
```Mỗi tờ có 3 chiếc máy bay nên một người cần`ceil(10 / 3) = 4`tờ. 

| Bước | Giá trị | 
| --- | --- | 
| k | 1 | 
| tờ mỗi người | 4 | 
| tổng số tờ | 4 | 
| p | 5 | 
| gói | 1 | 

Dù chỉ cần 4 tờ nhưng họ phải mua cả gói. Điều này cho thấy mức trần thứ hai chiếm ưu thế như thế nào khi kích thước gói hàng vượt quá nhu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học cố định được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Giải pháp này phù hợp một cách thoải mái trong các giới hạn vì nó thực hiện số học số nguyên theo thời gian không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    k, n, s, p = map(int, input().split())
    need_per_person = (n + s - 1) // s
    total_sheets = k * need_per_person
    packs = (total_sheets + p - 1) // p
    return str(packs)

# provided sample
assert run("5 3 2 3") == "4"

# minimum values
assert run("1 1 1 1") == "1"

# exact division no rounding needed
assert run("2 4 2 2") == "2"

# requires ceiling in first step
assert run("3 5 2 10") == "1"

# requires ceiling in second step
assert run("1 3 2 2") == "2"

# large balanced case
assert run("10000 10000 1 10000") == "10000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | 1 | trường hợp cạnh tối thiểu | 
| 2 4 2 2 | 2 | không cần làm tròn | 
| 3 5 2 10 | 1 | trần ở bước đầu tiên | 
| 1 3 2 2 | 2 | trần trong chuyển đổi gói | 
| 10000 10000 1 10000 | 10000 | xử lý quy mô tối đa | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một tờ giấy đã đủ cho một người. Đối với đầu vào:```
k=4, n=1, s=10, p=3
```Mỗi người cần`ceil(1/10)=1`tờ giấy. Tổng số tờ là 4, và gói là`ceil(4/3)=2`. Thuật toán tránh được việc đánh giá thấp một cách chính xác do các trang phân số. 

Một trường hợp khác là khi kích thước gói lớn hơn tổng yêu cầu về tờ giấy. Đối với đầu vào:```
k=1, n=5, s=2, p=100
```chúng tôi nhận được`ceil(5/2)=3`tờ, tổng cộng là 3 tờ. Dù chỉ cần 3 tờ nhưng phải mua một gói đầy đủ. Thuật toán tạo ra`ceil(3/100)=1`, xử lý chính xác tình trạng cung vượt cầu do độ chi tiết của gói gây ra.
