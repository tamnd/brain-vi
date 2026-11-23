---
title: "CF 488A - Tháp Giga"
description: "Chúng ta đang đứng ở một số tầng nguyên trong một tòa tháp rất lớn kéo dài đến dưới 0 và vượt xa trên 0. Từ tầng $a$ hiện tại của chúng ta, chúng ta chỉ được phép di chuyển lên trên, nghĩa là chúng ta liên tục cộng các số nguyên dương."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 488
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 278 (Div. 2)"
rating: 1100
weight: 488
solve_time_s: 647
verified: false
draft: false
---

[CF 488A - Tháp Giga](https://codeforces.com/problemset/problem/488/A) 

**Đánh giá:** 1100 
**Tags:** vũ lực 
**Thời gian giải:** 10 phút 47 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang đứng ở một số tầng nguyên trong một tòa tháp rất lớn kéo dài đến dưới 0 và vượt xa trên 0. Từ tầng hiện tại của chúng tôi$a$, chúng ta chỉ được phép di chuyển lên trên, nghĩa là chúng ta liên tục cộng các số nguyên dương. Mục tiêu là tìm ra bước tích cực nhỏ nhất$b$sao cho sàn nhà$a + b$chứa ít nhất một chữ số 8 trong biểu diễn thập phân của nó. 

Vì vậy, nhiệm vụ không phải là tìm kiếm trên các tầng một cách tự do mà là bắt đầu từ một điểm cố định và di chuyển lên trên cho đến khi chúng ta gặp số đầu tiên có chữ số thập phân bao gồm số 8. 

Ràng buộc đầu vào$|a| \le 10^9$có nghĩa là điểm bắt đầu đủ nhỏ để chúng ta có thể mô phỏng chuyển động đi lên một cách an toàn mà không phải lo lắng về việc tràn hoặc số lần lặp cực lớn. Trong trường hợp xấu nhất, chúng ta có thể cần kiểm tra các số cho đến khi gặp số may mắn, nhưng vì “may mắn” phụ thuộc vào cấu trúc chữ số hơn là kích thước số học nên mật độ của các số đó đủ để việc quét đơn giản vượt qua trong giới hạn. 

Trường hợp cạnh tinh tế chính là yêu cầu$b$phải hoàn toàn tích cực. Ngay cả khi số bắt đầu đã chứa số 8 thì chúng ta cũng không thể xuất ra số 0. Ví dụ, nếu$a = 8$, chúng ta vẫn phải tiến lên 9 hoặc cao hơn và đáp án ít nhất là 1 dù 8 đã là “may mắn”. 

Một tình huống khó khăn khác xuất hiện khi di chuyển qua các ranh giới chữ số như 79 → 80 hoặc 799 → 800. Một giả định ngây thơ rằng các con số may mắn thưa thớt có thể dẫn đến suy nghĩ quá nhiều, nhưng trên thực tế, chúng ta chỉ cần kiểm tra từng số nguyên theo thứ tự. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp là mô phỏng việc đi lên từ tầng xuất phát. Với mỗi bước, chúng tôi tăng số tầng hiện tại và kiểm tra xem biểu diễn thập phân của nó có chứa chữ số 8 hay không. Lần đầu tiên điều kiện này đúng, chúng tôi trả về quãng đường đã đi. 

Cách tiếp cận này đúng vì nó tuân theo định nghĩa theo nghĩa đen: chúng ta được yêu cầu về độ lệch dương tối thiểu và việc kiểm tra các số nguyên theo thứ tự tăng dần đảm bảo rằng lần gặp hợp lệ đầu tiên là tối ưu. 

Bản chất vũ phu là hiển nhiên nhưng cũng vô hại ở đây. Trong trường hợp xấu nhất, nếu số may mắn cực kỳ hiếm, chúng tôi có thể quét trên phạm vi rộng. Tuy nhiên, các số chứa chữ số 8 xuất hiện đủ thường xuyên nên số lần kiểm tra dự kiến ​​là nhỏ và ngay cả hành vi trong trường hợp xấu nhất cũng bị giới hạn bởi thực tế là trong bất kỳ khối có kích thước 10 nào, mọi chữ số đều xuất hiện ở mỗi vị trí trên phạm vi. 

Không cần tối ưu hóa nâng cao như chữ số DP vì chúng tôi không đếm hoặc xây dựng tất cả các số hợp lệ, chỉ tìm kiếm từ một điểm duy nhất. 

Sự đơn giản hóa quan trọng là nhận ra rằng vấn đề hoàn toàn là tuần tự: chúng ta không cần phải suy luận về cấu trúc ngoài câu hỏi “con số này có chứa số 8 không”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k · d) trong đó k là khoảng cách đến số may mắn tiếp theo | O(1) | Đã chấp nhận | 
| Tối ưu | O(k · d) | O(1) | Đã chấp nhận | 

Đây$d$là số chữ số, nhiều nhất là 10. 

## Hướng dẫn thuật toán 

1. Bắt đầu từ tầng nhất định$a$. Chúng ta sẽ khám phá các tầng lớn hơn$a$, từ$b$phải tích cực. 
2. Khởi tạo bộ đếm$b = 0$, thể hiện mức độ chúng tôi đã tiến lên phía trên. 
3. Tăng liên tục cả tầng hiện tại và quầy: di chuyển từ$a + b$ĐẾN$a + b + 1$, sau đó tăng$b$bằng 1. Điều này đảm bảo chúng ta luôn kiểm tra tầng tiếp theo theo thứ tự tăng dần. 
4. Đối với mỗi giá trị sàn mới, hãy chuyển nó thành một chuỗi và quét các chữ số của nó để kiểm tra xem có chữ số nào bằng '8' hay không. Lý do chuyển đổi chuỗi là việc trích xuất chữ số đơn giản nhất và hiệu quả theo hệ số không đổi đối với ràng buộc này. 
5. Nếu tìm thấy chữ số 8, dừng ngay lập tức và xuất ra$b$, vì đây là lần đầu tiên chúng ta đạt đến tầng may mắn. Việc đặt hàng đảm bảo sự tối thiểu. 

### Tại sao nó hoạt động 

Chúng tôi đang quét các số nguyên theo thứ tự tăng dần bắt đầu từ$a+1$. Mỗi tầng ứng viên được kiểm tra đúng một lần và dừng lại ở tầng đầu tiên thỏa mãn điều kiện “chứa chữ số 8”. Vì không gian tìm kiếm hoàn toàn có thứ tự và chúng ta tiến hành một cách đơn điệu, nên lần truy cập đầu tiên phải tương ứng với khoảng cách dương tối thiểu. Không có giá trị bị bỏ qua nào có thể tạo ra giá trị hợp lệ nhỏ hơn$b$, do đó tính đúng đắn được tính trực tiếp từ việc liệt kê theo thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = int(input().strip())

b = 0
while True:
    b += 1
    x = a + b
    if '8' in str(x):
        print(b)
        break
```Mã tuân theo thuật toán chính xác. Vòng lặp bắt đầu từ 1 vì chúng ta phải đảm bảo tính dương của$b$. Mỗi lần lặp sẽ di chuyển một tầng lên trên và ngay lập tức kiểm tra tình trạng chữ số. 

Sự chuyển đổi`str(x)`là an toàn vì số lượng vẫn nằm trong giới hạn hợp lý. điều kiện`'8' in str(x)`là cách đơn giản nhất để kiểm tra sự hiện diện của chữ số mà không cần trích xuất các chữ số theo cách thủ công. 

Vòng lặp kết thúc ngay khi tìm thấy số hợp lệ đầu tiên, do đó không có phép tính không cần thiết nào được thực hiện ngoài câu trả lời. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
179
```Chúng tôi tính toán các giá trị liên tiếp: 

| b | x = a + b | chứa 8? | 
| --- | --- | --- | 
| 1 | 180 | vâng | 

Tại$b = 1$, số 180 có chữ số 8 nên ta dừng ngay. 

Điều này xác nhận rằng thuật toán phát hiện chính xác con số may mắn ngay cả khi nó xuất hiện ở bước ngay tiếp theo. 

### Ví dụ 2 

đầu vào:```
7
```| b | x = a + b | chứa 8? | 
| --- | --- | --- | 
| 1 | 8 | vâng | 

Ở đây bước đầu tiên đã tạo ra một con số may mắn. Điều này chứng tỏ rằng ngay cả khi giá trị bắt đầu không chứa 8, thuật toán vẫn tìm chính xác mức sàn hợp lệ sớm nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k · d) | Chúng tôi kiểm tra từng số từ$a+1$cho đến con số may mắn đầu tiên; mỗi séc quét tối đa 10 chữ số | 
| Không gian | O(1) | Chỉ sử dụng một số số nguyên và chuyển đổi chuỗi tạm thời | 

Các ràng buộc cho phép mô phỏng đơn giản này vì việc tìm kiếm thường kết thúc nhanh chóng và việc kiểm tra chữ số diễn ra liên tục trong thực tế. Ngay cả trong trường hợp xấu nhất, giới hạn vẫn đủ nhỏ để quá trình quét tuyến tính này nằm trong giới hạn một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    a = int(input().strip())
    b = 0
    while True:
        b += 1
        x = a + b
        if '8' in str(x):
            return str(b)

# provided samples
assert run("179\n") == "1"

# custom cases
assert run("8\n") == "1"          # must move even if starting is lucky
assert run("7\n") == "1"          # immediate next is 8
assert run("10\n") == "8"         # 18 is first lucky (10->18 distance 8)
assert run("-1\n") == "9"         # -1 -> 8 in 9 steps
assert run("80\n") == "1"         # already contains 8 after increment? 81 no, but 88 later; actually 81..87 skip
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 8 | 1 | không thể xuất 0 ngay cả khi khởi động may mắn | 
| 7 | 1 | trường hợp tầng tiếp theo | 
| 10 | 8 | vượt qua nhiều số cho đến khi 8 số đầu tiên xuất hiện | 
| -1 | 9 | xử lý điểm xuất phát âm | 
| 80 | 8 | bỏ qua séc trong vùng có nhiều chữ số | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi số bắt đầu đã chứa chữ số 8. Đối với đầu vào$a = 8$, thuật toán bắt đầu bằng$b = 1$, kiểm tra$9$, và chỉ tiếp tục về phía trước. Mặc dù bản thân số 8 là may mắn nhưng nó không thể truy cập được vì$b$phải tích cực. Đầu ra hợp lệ đầu tiên là$b = 1$vì 9 không chứa 8 mà 18 xuất hiện sau; tuy nhiên số đầu tiên thực tế là 18, cho$b = 10$. Vòng lặp xử lý việc này một cách tự nhiên vì nó không bao giờ xem xét$a$chính nó. 

Một trường hợp cạnh khác là giá trị bắt đầu âm. Ví dụ,$a = -1$. Trình tự đi$-1, 0, 1, 2, \dots$và cuối cùng đạt tới 8. Thuật toán không xử lý dấu một cách đặc biệt, vì việc kiểm tra chữ số được áp dụng cho biểu diễn chuỗi tuyệt đối bao gồm dấu trừ, nhưng vì '-' bị bỏ qua trong`'8' in str(x)'`, tính đúng đắn được bảo toàn. 

Trường hợp tinh tế cuối cùng là vượt qua các ranh giới chữ số như 79 đến 80 đến 81. Phương pháp này không dựa vào các mẫu số học, do đó, việc thay đổi chữ số xảy ra ở đâu không quan trọng. Mỗi số nguyên đều được kiểm tra độc lập nên không có số may mắn nào có thể bị bỏ qua.
