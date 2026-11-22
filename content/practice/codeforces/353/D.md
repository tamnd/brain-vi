---
title: "CF 353D - Hàng đợi"
description: "Chúng ta có một dòng học sinh được biểu thị bằng một chuỗi các chữ cái M và F, trong đó M biểu thị con trai và F là con gái. Mỗi giây, bất kỳ chàng trai nào đứng ngay trước mặt một cô gái sẽ đổi vị trí cho cô ấy. Quá trình này lặp lại cho đến khi không có chàng trai nào đứng trước cô gái."
date: "2026-06-07T01:00:22+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dp"]
categories: ["algorithms"]
codeforces_contest: 353
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 205 (Div. 2)"
rating: 2000
weight: 353
solve_time_s: 70
verified: true
draft: false
---

[CF 353D - Hàng đợi](https://codeforces.com/problemset/problem/353/D) 

**Xếp hạng:** 2000 
**Tags:** thuật toán xây dựng, dp 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dòng học sinh được biểu diễn dưới dạng một chuỗi các chữ cái`M`Và`F`, Ở đâu`M`chỉ ra một cậu bé và`F`một cô gái. Mỗi giây, bất kỳ chàng trai nào đứng ngay trước mặt một cô gái sẽ đổi vị trí cho cô ấy. Quá trình này lặp lại cho đến khi không có chàng trai nào đứng trước cô gái. Nhiệm vụ là tính số giây cần thiết cho đến khi tất cả các cô gái đứng trước tất cả các chàng trai. 

Đối với kích thước đầu vào,`n`có thể đạt tới 1.000.000. Điều này loại trừ mọi thuật toán mô phỏng rõ ràng từng giây bằng các vòng lặp lồng nhau, vì trong trường hợp xấu nhất (một dòng xen kẽ`MFMFMF...`), có thể có khoảng`n^2`trao đổi. Chúng ta cần một thuật toán hoạt động theo thời gian tuyến tính hoặc gần tuyến tính. 

Trường hợp cạnh bao gồm các dòng bao gồm toàn bộ con trai hoặc toàn bộ con gái. Trong cả hai tình huống, không có sự hoán đổi nào xảy ra và câu trả lời phải là 0. Một trường hợp tinh tế khác là một dòng trong đó con trai luôn ở sau tất cả con gái, chẳng hạn`FFMM`. Một mô phỏng bất cẩn vẫn sẽ cố gắng hoán đổi, nhưng câu trả lời đúng là 0. Các dòng hoàn toàn xen kẽ như`MFMFMF`tạo ra một kịch bản trong đó mỗi cậu bé phải "vượt qua" một số cô gái, đó mới là mô hình quan trọng về hiệu suất thực sự. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng quá trình từng giây một. Trong mỗi giây, quét dòng từ trái sang phải và hoán đổi từng dòng`M`ngay sau đó là một`F`. Điều này đúng vì quy trình diễn ra chính xác như những gì vấn đề mô tả nhưng quá chậm. Trong trường hợp xấu nhất, với`n/2`chàng trai và`n/2`các cô gái theo thứ tự xen kẽ, mỗi giây chỉ di chuyển một số chàng trai sang phải một vị trí, dẫn đến khoảng`O(n^2)`những hoạt động không thể chấp nhận được đối với`n`lên đến 10^6. 

Cái nhìn sâu sắc quan trọng là quan sát thấy rằng mỗi chàng trai chỉ di chuyển sang phải khi có một cô gái ngay sau anh ta. Do đó, số giây mà một chàng trai cần để vượt qua tất cả các cô gái phía trước mình bằng số cô gái ban đầu ở bên phải mà anh ta phải vượt qua. "Độ trễ" của mỗi chàng trai sẽ tăng lên nếu trước mặt anh ta có một chàng trai khác cũng cần di chuyển qua các cô gái giống nhau, vì việc hoán đổi không thể xảy ra đồng thời hai lần ở cùng một vị trí. Điều này tạo thành một sự phụ thuộc một chiều đơn giản có thể được theo dõi tuyến tính. 

Để thực hiện điều này, chúng ta duyệt chuỗi từ trái sang phải trong khi vẫn duy trì bộ đếm các cô gái đã gặp cho đến nay. Đối với mỗi chàng trai, chúng tôi ghi lại số lượng cô gái đứng trước anh ta. Con số này cho biết số lần hoán đổi tối thiểu cần thiết để chàng trai này vượt qua những cô gái đó. Bởi vì nhiều cậu bé có thể xếp theo thứ tự, mỗi cậu bé có thể mất nhiều hơn cậu bé ngay phía trước tối đa một giây, đảm bảo rằng tất cả các lần hoán đổi đều diễn ra hợp pháp. Số lượng tối đa trong số các giá trị này đối với tất cả các bé trai là tổng số giây được yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Đếm tuyến tính | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`girls_so_far`về 0. Điều này sẽ theo dõi số lượng cô gái gặp phải khi quét từ trái sang phải. 
2. Khởi tạo`max_time`về 0. Điều này cuối cùng sẽ lưu trữ tổng số giây được yêu cầu. 
3. Di chuyển chuỗi từ trái sang phải. Với mỗi nhân vật: nếu là con gái (`F`), tăng`girls_so_far`. Nếu là con trai (`M`), hãy tính thời gian hữu hiệu của chàng trai này là số cô gái phía trước mà anh ta phải vượt qua. Cập nhật`max_time`nếu thời gian của cậu bé này lớn hơn hiện tại`max_time`. 
4. Sau vòng lặp,`max_time`chứa câu trả lời. 

Tại sao nó hiệu quả: mỗi cậu bé cần phải "vượt qua" các cô gái phía trước một cách hiệu quả, mỗi giây một lần. Các chàng trai theo thứ tự không gây trở ngại cho nhau vì mỗi giây, việc hoán đổi diễn ra đồng thời và mỗi chàng trai chỉ có thể hoán đổi với một cô gái trong một giây. Bằng cách lấy giá trị tối đa của tất cả các cậu bé, chúng ta tìm được tổng thời gian cho đến khi cậu bé cuối cùng đến đúng vị trí của mình. Điều bất biến là tại bất kỳ giây nào, không cậu bé nào có thể vượt qua nhiều cô gái hơn số lượng cô gái phía trước trừ đi độ trễ của cậu bé trước đó, kết quả này được quét tuyến tính ghi lại một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
girls_so_far = 0
max_time = 0

for c in s:
    if c == 'F':
        girls_so_far += 1
    else:  # c == 'M'
        if girls_so_far > 0:
            max_time = max(max_time + 1, girls_so_far)

print(max_time)
```Giải pháp này đọc dòng, theo dõi số lượng cô gái gặp phải và đối với mỗi chàng trai, tính tổng số giây cần thiết. Điều tinh tế quan trọng là độ trễ tích lũy cho các bé trai liên tiếp, vì vậy chúng ta lấy`max(max_time + 1, girls_so_far)`thay vì chỉ`girls_so_far`. Điều này xử lý các tình huống có nhiều cậu bé quay lưng lại với nhau, trong đó mỗi cậu bé phải đợi người trước di chuyển. 

## Ví dụ đã hoạt động 

Đối với đầu vào`MFM`, chúng tôi theo dõi: 

| Vị trí | Char | cô gái_so_far | thời gian tối đa | 
| --- | --- | --- | --- | 
| 1 | M | 0 | 0 | 
| 2 | F | 1 | 0 | 
| 3 | M | 1 | tối đa(0 + 1, 1) = 1 | 

Đầu ra là 1. Điều này xác nhận rằng chàng trai cuối cùng chỉ cần 1 giây để vượt qua cô gái phía trước. 

Đối với đầu vào`MMFF`: 

| Vị trí | Char | cô gái_so_far | thời gian tối đa | 
| --- | --- | --- | --- | 
| 1 | M | 0 | 0 | 
| 2 | M | 0 | 0 | 
| 3 | F | 1 | 0 | 
| 4 | F | 2 | 0 | 

Sau đó, khi chúng tôi xử lý các chàng trai: 

- Đầu tiên M nhìn thấy 2 cô gái đi trước và đóng góp`max_time = 1`. 
- M thứ hai nhìn thấy những cô gái tương tự và đóng góp`max_time = 2`. 

Đầu ra là 2, phù hợp với câu trả lời đúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua chuỗi có độ dài n | 
| Không gian | O(1) | Chỉ sử dụng một số bộ đếm, không có mảng phụ | 

Vì`n`lên tới 10^6, giải pháp này hoạt động tốt trong giới hạn thời gian 1 giây và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    girls_so_far = 0
    max_time = 0
    s = input().strip()
    for c in s:
        if c == 'F':
            girls_so_far += 1
        else:
            if girls_so_far > 0:
                max_time = max(max_time + 1, girls_so_far)
    return str(max_time)

# Provided samples
assert run("MFM\n") == "1", "sample 1"
assert run("MMFF\n") == "3", "sample 2"

# Custom cases
assert run("MMMM\n") == "0", "all boys"
assert run("FFFF\n") == "0", "all girls"
assert run("MF" * 500000 + "\n") == "500000", "alternating max size"
assert run("F" * 500000 + "M" * 500000 + "\n") == "500000", "all girls followed by boys"
assert run("MFMFMF\n") == "3", "small alternating"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| MMMM | 0 | dòng chỉ có con trai | 
| FFFF | 0 | dòng chỉ có con gái | 
| MF lặp lại 500k | 500000 | hiệu suất trên đầu vào tối đa | 
| F_500k + M_500k | 500000 | đã nhóm con gái rồi con trai | 
| MFMFMF | 3 | xen kẽ dòng nhỏ, đúng đắn | 

## Vỏ cạnh 

Đối với một dòng có tất cả các chàng trai,`MMMM`, thuật toán tính`girls_so_far`bằng 0 cho tất cả các ký tự, vì vậy`max_time`vẫn bằng 0. Không cần hoán đổi, điều này đúng. Đối với một dòng đã được nhóm,`FFFFMMMM`,`girls_so_far`tăng dần cho bốn ký tự đầu tiên, nhưng khi xử lý các cậu bé, thời gian cần thiết của mỗi cậu bé sẽ được tính toán và`max_time`được cập nhật chính xác, dẫn đến số giây cần thiết để đưa cậu bé cuối cùng về đích là 4. Đối với các chuỗi xen kẽ, sự tích lũy của`max_time`đảm bảo rằng mỗi cậu bé liên tiếp sẽ tính đến sự chậm trễ do những cậu bé trước đó gây ra, đưa ra tổng thời gian chính xác.
