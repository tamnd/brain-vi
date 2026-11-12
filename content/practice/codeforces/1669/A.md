---
title: "CF 1669A - Sư đoàn?"
description: "Mỗi trường hợp thử nghiệm đưa ra một số nguyên duy nhất biểu thị xếp hạng của người dùng Codeforces và nhiệm vụ là phân loại xếp hạng đó thành một trong bốn khoảng cố định, mỗi khoảng tương ứng với một số chia. Việc ánh xạ hoàn toàn dựa trên ngưỡng."
date: "2026-06-10T01:54:56+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1669
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 784 (Div. 4)"
rating: 800
weight: 1669
solve_time_s: 93
verified: true
draft: false
---

[CF 1669A - Phân chia?](https://codeforces.com/problemset/problem/1669/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm đưa ra một số nguyên duy nhất biểu thị xếp hạng của người dùng Codeforces và nhiệm vụ là phân loại xếp hạng đó thành một trong bốn khoảng cố định, mỗi khoảng tương ứng với một số chia. 

Việc ánh xạ hoàn toàn dựa trên ngưỡng. Xếp hạng rất thấp, bao gồm cả tiêu cực, rơi vào Phân khu 4. Khi xếp hạng tăng lên, nó vượt qua hai dải trung gian trước khi đạt Phân khu 1 để có xếp hạng cao nhất. Đầu ra không phải là một phép tính mà là một nhãn bắt nguồn từ vị trí của số nằm trong phân vùng có thứ tự của dòng số nguyên. 

Các ràng buộc rất lớn về số lượng trường hợp thử nghiệm, lên tới mười nghìn. Mỗi trường hợp thử nghiệm đều độc lập và chỉ yêu cầu một lượng công việc không đổi, do đó, giải pháp khả thi duy nhất là những giải pháp xử lý từng đầu vào trong thời gian O(1). Bất cứ điều gì liên quan đến việc sắp xếp hoặc quét lặp lại sẽ là chi phí không cần thiết nhưng vẫn có thể thực hiện được về mặt kỹ thuật trong giới hạn; tuy nhiên, nó sẽ không phù hợp về mặt khái niệm và chậm hơn mức cần thiết. 

Sự tinh tế duy nhất trong vấn đề này đến từ việc xử lý ranh giới. Các ranh giới phân chia mang tính bao hàm, do đó, các giá trị chính xác bằng 1400, 1600 và 1900 phải được đặt chính xác trong các dải phân chia cao hơn. Một sai lầm điển hình là sử dụng các bất đẳng thức nghiêm ngặt một cách không nhất quán, điều này có thể chuyển các giá trị biên sang phép chia sai. Ví dụ: coi 1400 là Phân khu 4 vì một`< 1400`kiểm tra thay vì`<= 1399`dẫn đến việc phân loại không chính xác. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ là kiểm tra rõ ràng từng khoảng phân chia theo trình tự và kiểm tra tư cách thành viên. Vì chỉ có bốn phạm vi nên ngay cả một cách tiếp cận đơn giản vẫn duy trì thời gian không đổi cho mỗi trường hợp thử nghiệm. Người ta có thể tưởng tượng việc lưu trữ các phạm vi theo cặp và lặp qua chúng, kiểm tra xem xếp hạng có rơi vào từng khoảng hay không. Điều này hoạt động chính xác vì số lượng khoảng thời gian là cố định và nhỏ, do đó công việc trong trường hợp xấu nhất trên mỗi trường hợp kiểm thử vẫn không đổi. 

Tuy nhiên, cấu trúc của bài toán đơn giản hơn so với thành viên khoảng tổng quát. Các khoảng liền kề nhau và được sắp xếp trên trục số, vì vậy chúng ta không cần lặp lại hoặc cấu trúc dữ liệu. Một chuỗi so sánh trực tiếp từ ngưỡng thấp nhất đến ngưỡng cao nhất là đủ. Sau khi phát hiện đúng băng tần, chúng tôi có thể trả về ngay nhãn phân chia tương ứng. 

Quan sát quan trọng là không gian xếp hạng được phân chia thành bốn vùng đơn điệu được xác định bởi ba điểm cắt: 1400, 1600 và 1900. Điều này biến vấn đề thành một vấn đề phân loại ngưỡng đơn giản, trong đó chúng ta chỉ cần so sánh với các điểm cắt này theo thứ tự tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (quét theo khoảng thời gian) | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 
| Tối ưu (kiểm tra ngưỡng) | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Điều này xác định số lượng phân loại độc lập mà chúng tôi sẽ thực hiện. 
2. Đối với mỗi xếp hạng, hãy so sánh xếp hạng đó với ngưỡng nhỏ nhất xác định mức phân chia cao nhất. Nếu xếp hạng ít nhất là 1900 thì nó phải thuộc Phân khu 1 vì không tồn tại ranh giới cao hơn. 
3. Nếu xếp hạng dưới 1900, hãy kiểm tra xem nó có ít nhất là 1600 hay không. Nếu vậy, nó nằm ở dải cao thứ hai vì nó được đảm bảo không đạt đến Phân khu 1. 
4. Nếu xếp hạng dưới 1600, hãy kiểm tra xem nó có ít nhất là 1400 hay không. Điều này xác định dải thứ ba vì các khả năng cao hơn đã bị loại trừ. 
5. Nếu không đáp ứng được điều kiện nào ở trên thì xếp hạng tối đa phải là 1399, xếp vào Phân khu 4. 

Thứ tự kiểm tra rất quan trọng vì mỗi điều kiện sẽ loại bỏ tiền tố của trục số. Khi một điều kiện thành công, các lần kiểm tra sau đó sẽ không còn phù hợp và không được ghi đè lên quyết định. 

### Tại sao nó hoạt động 

Dòng đánh giá được chia thành bốn khoảng rời rạc mà sự kết hợp của nó bao gồm tất cả các số nguyên. Mỗi kiểm tra có điều kiện tương ứng với tư cách thành viên trong một trong các khoảng này sau khi loại trừ các khoảng cao hơn. Vì quá trình kiểm tra tiến hành từ ngưỡng cao nhất đến ngưỡng thấp nhất nên mỗi xếp hạng được chỉ định cho chính xác một bộ phận và không thể xảy ra sự chồng chéo hoặc khoảng cách do các định nghĩa ranh giới bao gồm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    r = int(input())
    
    if r >= 1900:
        print("Division 1")
    elif r >= 1600:
        print("Division 2")
    elif r >= 1400:
        print("Division 3")
    else:
        print("Division 4")
```Giải pháp đọc từng xếp hạng một cách độc lập và áp dụng một chuỗi so sánh cố định. Điều kiện đầu tiên xử lý khoảng trên cùng và mỗi nhánh tiếp theo chỉ xử lý phạm vi thấp hơn tiếp theo sau khi loại trừ tất cả các phạm vi cao hơn. Điều này đảm bảo tính chính xác mà không cần mã hóa rõ ràng các điểm cuối khoảng. 

Một lỗi phổ biến là đảo ngược thứ tự kiểm tra, ví dụ như kiểm tra`r >= 1400`Đầu tiên. Điều đó sẽ phân loại không chính xác tất cả các xếp hạng cao hơn vào Phân khu 3 hoặc thấp hơn trừ khi được bảo vệ cẩn thận. Thứ tự giảm dần ngăn chặn sự phân loại sai như vậy. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi logic phân loại trên đầu vào mẫu. 

### Ví dụ 1 

đầu vào:```
-789
1299
1300
1399
1400
1679
2300
```| Đánh giá | r ≥ 1900 | r ≥ 1600 | r ≥ 1400 | Đầu ra | 
| --- | --- | --- | --- | --- | 
| -789 | Không | Không | Không | Phân khu 4 | 
| 1299 | Không | Không | Không | Phân khu 4 | 
| 1300 | Không | Không | Không | Phân khu 4 | 
| 1399 | Không | Không | Không | Phân khu 4 | 
| 1400 | Không | Không | Có | Phân khu 3 | 
| 1679 | Không | Có | - | Phân khu 2 | 
| 2300 | Có | - | - | Phân khu 1 | 

Dấu vết này cho biết mỗi xếp hạng được lọc theo các ngưỡng như thế nào cho đến khi áp dụng chính xác một nhánh. Nó cũng nêu bật lý do tại sao thứ tự kiểm tra ngăn chặn sự mơ hồ ở các giá trị biên như 1400 và 1600. 

### Ví dụ 2 

đầu vào:```
4
1600
1599
1900
-5000
```| Đánh giá | r ≥ 1900 | r ≥ 1600 | r ≥ 1400 | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1600 | Không | Có | - | Phân khu 2 | 
| 1599 | Không | Không | Có | Phân khu 3 | 
| 1900 | Có | - | - | Phân khu 1 | 
| -5000 | Không | Không | Không | Phân khu 4 | 

Ví dụ này tập trung vào các giá trị liền kề với ranh giới, xác nhận rằng sự bằng nhau ở các ngưỡng được xử lý chính xác nhờ so sánh toàn diện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu số lượng so sánh không đổi và một đầu ra. | 
| Không gian | O(1) | Không có cấu trúc dữ liệu phụ trợ nào được sử dụng ngoài các biến để phân tích cú pháp đầu vào. | 

Giải pháp có tỷ lệ tuyến tính theo số lượng ca kiểm thử, là mức tối ưu vì mỗi xếp hạng phải được đọc và phân loại ít nhất một lần. Với tối đa 10.000 trường hợp, điều này dễ dàng phù hợp với giới hạn thời gian trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        r = int(input())
        if r >= 1900:
            out.append("Division 1")
        elif r >= 1600:
            out.append("Division 2")
        elif r >= 1400:
            out.append("Division 3")
        else:
            out.append("Division 4")
    return "\n".join(out) + "\n"

# provided sample
assert run("""7
-789
1299
1300
1399
1400
1679
2300
""") == """Division 4
Division 4
Division 4
Division 4
Division 3
Division 2
Division 1
"""

# minimum value
assert run("""1
-5000
""") == "Division 4\n"

# boundary checks
assert run("""3
1399
1400
1599
""") == """Division 4
Division 3
Division 3
"""

# higher boundaries
assert run("""3
1600
1899
1900
""") == """Division 2
Division 2
Division 1
"""

# mixed values
assert run("""4
0
1000
2000
1800
""") == """Division 4
Division 4
Division 1
Division 2
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| -5000 | Phân khu 4 | xử lý ranh giới tối thiểu | 
| 1399/1400/1599 | D4/D3/D3 | độ chính xác của ranh giới dưới và giữa | 
| 1600/1899/1900 | D2/D2/D1 | độ đúng ranh giới trên | 
| giá trị hỗn hợp | đa dạng | độ chính xác chung trên tất cả các băng tần | 

## Vỏ cạnh 

Xếp hạng tiêu cực chứng minh rằng dải thấp nhất vượt quá 0 thành các số âm không giới hạn. Đối với một đầu vào như`-1`, séc`r >= 1900`,`r >= 1600`, Và`r >= 1400`tất cả đều thất bại, để lại Phân khu 4 là phân loại chính xác. Thuật toán xử lý việc này một cách tự nhiên vì nhánh else cuối cùng nắm bắt mọi thứ không nằm trong ngưỡng cao hơn. 

Các giá trị biên như 1400, 1600 và 1900 rất quan trọng vì chúng nằm chính xác trên các chuyển tiếp phân chia. Ví dụ: 1400 phải vượt qua`r >= 1400`kiểm tra, đảm bảo Phân khu 3, trong khi 1399 không đạt tất cả các bước kiểm tra cao hơn và rơi vào Phân khu 4. Thứ tự nghiêm ngặt của các điều kiện đảm bảo vị trí chính xác mà không yêu cầu điểm cuối khoảng thời gian rõ ràng.
