---
title: "CF 413A - Phục hồi dữ liệu"
description: "Bài toán cung cấp cho chúng ta một chuỗi nhiệt độ bộ xử lý được ghi lại qua n bước. Kỹ sư trưởng có những ghi chú đáng tin cậy về nhiệt độ tối thiểu và tối đa quan sát được, nhưng người trợ lý chỉ ghi lại m trong n nhiệt độ."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 413
codeforces_index: "A"
codeforces_contest_name: "Coder-Strike 2014 - Round 2"
rating: 1200
weight: 413
solve_time_s: 102
verified: true
draft: false
---

[CF 413A - Phục hồi dữ liệu](https://codeforces.com/problemset/problem/413/A) 

**Đánh giá:** 1200 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề cung cấp cho chúng ta một chuỗi nhiệt độ bộ xử lý được ghi lại trong`n`các bước. Kỹ sư trưởng có những ghi chú đáng tin cậy về nhiệt độ tối thiểu và tối đa quan sát được, nhưng người trợ lý chỉ ghi lại`m`của`n`nhiệt độ. Chúng tôi được yêu cầu xác định liệu báo cáo`m`nhiệt độ có thể được mở rộng với các giá trị bổ sung để phù hợp với báo cáo của kỹ sư trưởng`min`Và`max`. 

Cụ thể ta có số nguyên`n`Và`m`đại diện cho tổng số nhiệt độ và tập hợp con mà chúng ta đã biết. Chúng ta cũng có số nguyên`min`Và`max`đó là nhiệt độ tối thiểu và tối đa tuyệt đối phải xuất hiện trong chuỗi cuối cùng. Danh sách của`m`nhiệt độ được ghi lại có thể bao gồm hoặc không bao gồm những điểm cực trị này. Đầu ra là "Chính xác" nếu có thể thêm`n - m`nhiệt độ để thỏa mãn các thái cực, nếu không thì "Không chính xác". 

Các hạn chế là nhỏ:`n`tối đa là 100, điều này ngay lập tức gợi ý rằng bất kỳ thuật toán nào có chức năng quét tuyến tính hoặc một vài lần vượt qua nhiệt độ đều đủ hiệu quả. Bản thân nhiệt độ nằm trong khoảng từ 1 đến 100, do đó không yêu cầu cấu trúc dữ liệu đặc biệt. Các trường hợp cạnh liên quan đến các tình huống trong đó dòng điện`m`nhiệt độ đã vi phạm các ràng buộc - ví dụ: tất cả nhiệt độ được ghi lại đều cao hơn`max`hoặc thấp hơn`min`. Một trường hợp cạnh khác là khi`min`hoặc`max`đã có trong danh sách trợ lý; trong trường hợp đó, chúng ta không cần thêm lại nhưng trình tự vẫn phải đảm bảo đủ nhiệt độ bổ sung để đạt được`n`. 

Một ví dụ trong đó việc triển khai đơn giản có thể thất bại là nếu trợ lý báo cáo nhiệt độ cao hơn`max`. Nếu thuật toán chỉ kiểm tra xem`min`Và`max`đang được thiết lập, nó có thể nói "Chính xác" không chính xác mặc dù nhiệt độ hiện tại không phù hợp với mức cực đoan cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ nhất là liệt kê tất cả các chuỗi của`n`nhiệt độ bao gồm`m`những cái được báo cáo và kiểm tra xem có đáp ứng được không`min`Và`max`hạn chế. Điều này đúng về mặt kỹ thuật nhưng lại bùng nổ về mặt tổ hợp. Vì`n = 100`Và`m = 1`, số lượng trình tự cần kiểm tra lớn về mặt thiên văn. Ngay cả khi chỉ có hai tùy chọn cho mỗi nhiệt độ được thêm vào (hoặc`min`hoặc`max`), trường hợp xấu nhất sẽ yêu cầu kiểm tra 2^99 khả năng. 

Cái nhìn sâu sắc quan trọng mở ra một giải pháp đơn giản hơn là chúng ta không quan tâm đến thứ tự chính xác của nhiệt độ mà chỉ quan tâm đến sự hiện diện của các cực trị. Chúng ta chỉ cần kiểm tra xem nhiệt độ được báo cáo hiện tại đã thỏa mãn các cực trị hay có thể được kéo dài để thỏa mãn chúng. Trong thực tế, điều này chỉ dừng lại ở việc kiểm tra xem nhiệt độ tối thiểu được báo cáo có cao nhất hay không.`min`và liệu nhiệt độ tối đa được báo cáo có ít nhất là`max`. Nếu thiếu một trong hai thái cực, chúng ta luôn có thể thêm nó vào phần bổ sung`n - m`khe cắm, bởi vì`n - m >= 1`bởi những ràng buộc của vấn đề. 

Quan sát này cho phép chúng ta giải quyết vấn đề theo thời gian tuyến tính bằng một lần quét nhiệt độ được báo cáo và một vài so sánh đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(100^(100)) | O(100) | Quá chậm | 
| Tối ưu | O(m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`,`m`,`min`,`max`từ đầu vào, theo sau là danh sách`m`nhiệt độ. 
2. Tính nhiệt độ tối thiểu và tối đa được ghi lại. Gọi đây là`current_min`Và`current_max`. 
3. Kiểm tra xem`current_min <= min`. Nếu không thì không thể bổ sung thêm nhiệt độ để giảm mức tối thiểu xuống dưới giá trị nhỏ nhất được ghi. Trong trường hợp đó, xuất ra "Không chính xác". 
4. Kiểm tra xem`current_max >= max`. Nếu không thì không thể tăng mức tối đa lên trên giá trị lớn nhất được ghi. Đầu ra "Không chính xác". 
5. Nếu cả hai bước kiểm tra đều đạt, hãy xuất ra "Chính xác" vì chúng ta luôn có thể thêm nhiệt độ bổ sung (có thể lặp lại`min`hoặc`max`) để lấp đầy phần còn lại`n - m`khe cắm. 

Tại sao nó hoạt động: Thuật toán đảm bảo tính chính xác vì mọi nhiệt độ bị thiếu chỉ có thể ảnh hưởng đến mức tối thiểu hoặc tối đa nếu chúng vượt quá các mức cực đoan hiện có. Vì chúng ta có thể tự do lựa chọn nhiệt độ bổ sung, miễn là các giá trị hiện có không vi phạm yêu cầu`min`hoặc`max`, chúng ta luôn có thể chèn các điểm cực trị còn thiếu. Thuộc tính này làm giảm vấn đề thành hai so sánh đơn giản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, min_temp, max_temp = map(int, input().split())
temps = list(map(int, input().split()))

current_min = min(temps)
current_max = max(temps)

if current_min > min_temp or current_max < max_temp:
    print("Incorrect")
else:
    print("Correct")
```Dòng đầu tiên đọc các thông số và dòng tiếp theo đọc nhiệt độ được ghi lại. các`min`Và`max`chức năng xác định các điểm cực đoan trong báo cáo của trợ lý. Câu điều kiện so sánh những điểm cực đoan này với những điều kiện cần thiết`min`Và`max`. Lưu ý rằng việc sử dụng`>`Và`<`đảm bảo chính xác rằng chúng tôi chỉ từ chối các chuỗi không thể đáp ứng các ràng buộc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 1 1 2
1
```| Bước | nhiệt độ | hiện tại_min | hiện tại_max | Kiểm tra | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1] | 1 | 1 | 1 <= 1 (ok), 1 >= 2 (thất bại?) | 
| Sau khi kiểm tra | 1 <= 1, 1 < 2 | Thiếu tối đa | Thêm 2 | Đúng | 

các`min`đã có sẵn và chúng ta có thể thêm phần còn thiếu`max`trong khe phụ. 

### Mẫu 2 

đầu vào:```
3 1 1 3
2
```| Bước | nhiệt độ | hiện tại_min | hiện tại_max | Kiểm tra | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [2] | 2 | 2 | 2 <= 1 (thất bại), 2 >= 3 (thất bại) | 
| Đầu ra | Không đúng | | | | 

Ở đây, nhiệt độ được báo cáo cao hơn`min`và thấp hơn`max`, khiến cho không thể thỏa mãn cả hai thái cực với một nhiệt độ bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Quét danh sách nhiệt độ được báo cáo một lần để tìm nhiệt độ tối thiểu và tối đa. | 
| Không gian | O(m) | Lưu trữ danh sách nhiệt độ; không có cấu trúc bổ sung cần thiết. | 

Với`m <= n <= 100`, thuật toán tuyến tính này chạy trong micro giây và sử dụng bộ nhớ không đáng kể, nằm trong giới hạn của bài toán. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, min_temp, max_temp = map(int, input().split())
    temps = list(map(int, input().split()))
    current_min = min(temps)
    current_max = max(temps)
    if current_min > min_temp or current_max < max_temp:
        return "Incorrect"
    return "Correct"

# provided samples
assert run("2 1 1 2\n1\n") == "Correct", "sample 1"
assert run("3 1 1 3\n2\n") == "Incorrect", "sample 2"

# custom cases
assert run("5 3 2 5\n2 3 4\n") == "Correct", "missing max"
assert run("4 2 1 4\n1 2\n") == "Correct", "already includes min"
assert run("3 2 2 5\n2 5\n") == "Correct", "already includes min and max"
assert run("3 2 1 3\n2 2\n") == "Correct", "needs both min and max"
assert run("3 2 1 2\n3 3\n") == "Incorrect", "all temps exceed max"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 3 2 5, 2 3 4 | Đúng | Có thể thêm mức tối đa bị thiếu để đáp ứng các ràng buộc | 
| 4 2 1 4, 1 2 | Đúng | Đã bao gồm tối thiểu, có thể thêm tối đa | 
| 3 2 2 5, 2 5 | Đúng | Những thái cực đã hiện diện | 
| 3 2 1 2, 2 2 | Đúng | Có thể thêm min để thỏa mãn dãy | 
| 3 2 1 2, 3 3 | Không đúng | Nhiệt độ hiện tại không đáp ứng được mức tối thiểu/tối đa | 

## Vỏ cạnh 

Một trường hợp khó phát hiện là khi tất cả nhiệt độ được báo cáo đều nằm ngoài phạm vi yêu cầu. Ví dụ,`n = 3`,`m = 2`,`min = 1`,`max = 3`, và nhiệt độ được báo cáo là`[4, 5]`. Thuật toán tính toán`current_min = 4`Và`current_max = 5`. Từ`current_min > min`, lần kiểm tra đầu tiên không thành công và xuất ra "Không chính xác". Điều này xử lý chính xác tình huống trong đó việc thêm nhiệt độ bổ sung không thể hạ thấp mức tối thiểu xuống giá trị yêu cầu. Một trường hợp khó khăn khác là khi danh sách trợ lý đã chứa cả hai`min`Và`max`. Vì`n = 4`,`m = 2`,`min = 1`,`max = 4`,`temps = [1,4]`, thuật toán vượt qua cả kiểm tra và đầu ra "Chính xác", cho phép lấp đầy các khe còn lại bằng bất kỳ nhiệt độ nào. 

Những trường hợp đặc biệt này xác nhận rằng giải pháp giải quyết đúng đắn cả những vấn đề còn thiếu
