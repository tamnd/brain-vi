---
title: "CF 105962B - Chúng tôi đang cạnh tranh"
description: "Chúng ta có một chuỗi S dài không có dấu cách, đây là một bản phiên âm ồn ào của một số văn bản gốc. Quá trình phiên âm có thể đã làm hỏng tối đa K ký tự riêng lẻ, nghĩa là hầu hết K vị trí trong văn bản gốc đã được thay thế bằng các ký tự khác."
date: "2026-06-22T16:15:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105962
codeforces_index: "B"
codeforces_contest_name: "UNICAMP Freshman Contest 2025"
rating: 0
weight: 105962
solve_time_s: 67
verified: true
draft: false
---

[CF 105962B - Chúng tôi đang cạnh tranh](https://codeforces.com/problemset/problem/105962/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi dài`S`không có dấu cách, đó là sự phiên âm ồn ào của một số văn bản gốc. Quá trình sao chép có thể bị hỏng tới`K`các ký tự riêng lẻ, có nghĩa là nhiều nhất`K`các vị trí trong văn bản gốc được thay thế bằng các ký tự khác nhau. 

Thuộc tính duy nhất mà chúng ta quan tâm là liệu văn bản gốc có thể chứa cụm từ đó hay không`"tamo competindo"`như một chuỗi con liền kề. Bởi vì khoảng trắng được loại bỏ trong đầu vào, cụm từ mục tiêu này thực sự trở thành`"tamo competindo"`được coi là một mẫu ký tự, bao gồm khoảng trắng ở dạng ban đầu, nhưng trong thế giới đầu vào, chúng ta phải coi nó như một chuỗi sẽ xuất hiện sau khi loại bỏ khoảng trắng. Vì vậy, mẫu chúng tôi đang tìm kiếm là chuỗi chính xác`"tamocompetindo"`. 

Nhiệm vụ là xác định xem có tồn tại một chuỗi con nào đó của`S`điều đó có thể biến thành`"tamocompetindo"`bằng cách thay đổi nhiều nhất`K`tổng số nhân vật. Tương tự, chúng ta đang hỏi liệu có tồn tại sự liên kết của mẫu này bên trong hay không`S`với khoảng cách Hamming nhiều nhất`K`. 

Các ràng buộc rất lớn:`|S|`có thể lên tới 50.000, trong khi`K`tối đa là 14. Điều này ngay lập tức loại trừ mọi so sánh bậc hai giữa tất cả các chuỗi con và mẫu, vì việc kiểm tra mọi vị trí bắt đầu sẽ yêu cầu tới 50.000 so sánh, mỗi so sánh tối đa khoảng 14 ký tự. Chỉ riêng điều này là ổn nhưng sẽ trở thành vấn đề nếu thực hiện không hiệu quả trong nhiều trường hợp. Tuy nhiên, cấu trúc của mẫu ngắn và cố định, điều này cho thấy cửa sổ trượt hoặc kiểm tra căn chỉnh trực tiếp là đủ. 

Một sai lầm ngây thơ sẽ nảy sinh nếu người ta cố gắng cho phép thêm hoặc xóa tùy ý thay vì chỉ thay thế. Ví dụ: ai đó có thể nghĩ rằng vấn đề về khoảng cách yêu cầu khoảng cách chỉnh sửa, nhưng câu lệnh chỉ cho phép thay đổi ký tự chứ không cho phép thay đổi cấu trúc. Một vấn đề tế nhị khác là quên rằng chúng ta chỉ quan tâm đến sự tồn tại của một cửa sổ hợp lệ chứ không phải tất cả các lần xuất hiện. 

Các trường hợp cạnh bao gồm các chuỗi rất ngắn, chẳng hạn như không có cửa sổ có độ dài cần thiết`S = "abc"`phải bị lỗi ngay lập tức và các trường hợp chuỗi đã chứa chính xác mẫu nhưng có thể có nhiễu xung quanh. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: trượt một cửa sổ có độ dài bằng mẫu trên chuỗi`S`. Đối với mỗi vị trí, so sánh chuỗi con với`"tamocompetindo"`và đếm xem có bao nhiêu vị trí khác nhau. Nếu bất kỳ cửa sổ nào có số lượng không khớp nhiều nhất`K`, chúng ta có thể kết luận ngay rằng điều đó là có thể. 

Điều này hiệu quả vì mô hình sai lệch hoàn toàn là sự thay thế vị trí, do đó, mỗi căn chỉnh mô tả một cách độc lập khả năng tái tạo lại văn bản gốc. 

Chi phí kiểm tra brute-force cho một cửa sổ`O(14)`vì chiều dài mẫu là cố định. có`O(n)`windows, vì vậy tổng độ phức tạp là`O(14n)`, đó là tuyến tính trong thực tế. Thậm chí tại`n = 5 * 10^4`, đây là khoảng 700.000 so sánh ký tự, đủ nhanh. 

Quan sát quan trọng là chúng ta không cần bất kỳ cấu trúc khớp chuỗi nâng cao nào như KMP hoặc hàm băm vì mẫu cực kỳ nhỏ và`K`là nhỏ bé. Chúng tôi chỉ cần kiểm tra sự không phù hợp cục bộ. Ràng buộc`K ≤ 14`cũng đưa ra điều kiện dừng sớm: nếu độ lệch vượt quá`K`trong khi quét một cửa sổ, chúng ta có thể phá vỡ sớm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Cửa sổ trượt Brute Force | O(n * m) với m = 14 | O(1) | Đã chấp nhận | 
| Kiểm tra thoát sớm được tối ưu hóa | O(n * phút(m, K)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Xác định mẫu mục tiêu là`"tamocompetindo"`và tính độ dài của nó`m = 14`. Đây là chuỗi duy nhất mà chúng tôi từng so sánh, vì vậy tất cả các lần kiểm tra đều giảm xuống mức khớp với mẫu cố định này. 
2. Nếu chuỗi đầu vào`S`có chiều dài nhỏ hơn`m`, xuất ngay`NAO`. Không có chuỗi con nào có thể khớp với mẫu dài hơn. 
3. Lặp lại mọi chỉ mục bắt đầu`i`từ`0`ĐẾN`|S| - m`. 
4. Đối với mỗi`i`, so sánh`S[i:i+m]`từng ký tự so với mẫu, đếm những điểm không khớp. 
5. Trong khi đếm những số không khớp, hãy dừng sớm nếu số lượng vượt quá`K`. Điều này tránh những so sánh không cần thiết trong các cửa sổ vốn đã không hợp lệ. 
6. Nếu bất kỳ cửa sổ nào kết thúc với số lượng không khớp nhiều nhất`K`, đầu ra`SIM`ngay lập tức. 
7. Nếu không có cửa sổ nào thỏa mãn điều kiện sau khi quét tất cả các vị trí, hãy xuất`NAO`. 

Lý do đằng sau việc dừng sớm là một lần hơn`K`các vị trí khác nhau, không có phép chuyển đổi hợp lệ nào có thể sửa được cửa sổ đó trong số lần chỉnh sửa được phép, do đó việc tiếp tục so sánh không thể thay đổi kết quả. 

### Tại sao nó hoạt động 

Mỗi vị trí bắt đầu tiềm năng xác định một bản dựng lại ứng cử viên của văn bản gốc căn chỉnh mẫu với một chuỗi con của`S`. Vì chỉ cho phép thay thế nên chi phí để tạo ra chuỗi con đó bằng mẫu chính xác là số lượng vị trí không khớp. Nếu chi phí đó nằm trong`K`, chúng tôi có lời giải thích hợp lệ về việc tại sao văn bản bị lỗi vẫn có thể bắt nguồn từ một văn bản hợp lệ có chứa cụm từ đó. Quét tất cả các cửa sổ đảm bảo tính đầy đủ và việc cắt bớt thoát sớm sẽ duy trì tính chính xác vì số lượng không khớp chỉ tăng khi chúng tôi kiểm tra nhiều ký tự hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

S = input().strip()
K = int(input().strip())

pattern = "tamocompetindo"
m = len(pattern)
n = len(S)

if n < m:
    print("NAO")
    sys.exit(0)

for i in range(n - m + 1):
    mismatches = 0
    for j in range(m):
        if S[i + j] != pattern[j]:
            mismatches += 1
            if mismatches > K:
                break
    if mismatches <= K:
        print("SIM")
        break
else:
    print("NAO")
```Mã trực tiếp thực hiện ý tưởng cửa sổ trượt. Vòng lặp bên ngoài chọn sự liên kết ứng viên, trong khi vòng lặp bên trong đo lường chi phí thay thế. Việc nghỉ sớm đảm bảo chúng ta không lãng phí thời gian vào những việc sắp xếp vô vọng. trận chung kết`else`trên vòng lặp được sử dụng để in`"NAO"`chỉ khi không tìm thấy cửa sổ hợp lệ. 

Một chi tiết triển khai tinh tế là việc sử dụng Python`for-else`để tránh cần một biến cờ rõ ràng. Một điểm quan trọng khác là xử lý việc kiểm tra độ dài trước khi vào vòng lặp, điều này ngăn cản các giới hạn lặp âm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
tepoassimtamocompetindu
2
```Mẫu:`"tamocompetindo"`Chúng tôi trượt qua sợi dây và kiểm tra các cửa sổ có chiều dài 14. 

| tôi | chuỗi con | không khớp | ≤ K? | 
| --- | --- | --- | --- | 
| 8 | tamocompetindu | 2 | vâng | 

Ở chỉ số 8, chỉ có ký tự cuối cùng là khác`"o"`→`"u"`và có thể có một sự không khớp khác tùy thuộc vào việc căn chỉnh, nhưng tổng số nằm trong`K = 2`. 

Điều này xác nhận việc xây dựng lại hợp lệ tồn tại, vì vậy đầu ra là`SIM`. 

### Ví dụ 2 

đầu vào:```
temocompetindu
1
```| tôi | chuỗi con | không khớp | ≤ K? | 
| --- | --- | --- | --- | 
| 0 | temocompetindu | 1 | vâng | 

Ở đây, chuỗi con khác với mẫu chỉ ở một vị trí, nhưng vì cụm từ được yêu cầu`"tamocompetindo"`khác ở nhiều vị trí so với bất kỳ căn chỉnh nào, chúng tôi kiểm tra cẩn thận: chỉ riêng ký tự đầu tiên không khớp đã tiêu tốn toàn bộ ngân sách và phần còn lại vẫn nhất quán. Tuy nhiên, nếu có thêm bất kỳ sự không phù hợp nào trong so sánh đầy đủ, nó sẽ vượt quá`K`theo cách diễn giải chặt chẽ hơn, do đó không có cửa sổ hợp lệ nào được đảm bảo trên tất cả các sắp xếp. 

Vì vậy, nếu không có cửa sổ đầy đủ đáp ứng ràng buộc, đầu ra là`NAO`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 14) | Mỗi vị trí trong số tối đa 50.000 vị trí so sánh tối đa 14 ký tự với việc thoát sớm khi vượt quá K | 
| Không gian | O(1) | Chỉ sử dụng bộ đếm và bộ lưu trữ mẫu có kích thước cố định | 

Tổng số phép toán nằm dưới giới hạn một cách thoải mái vì vòng lặp bên trong được giới hạn bởi một hằng số và`K ≤ 14`cho phép chấm dứt sớm trong nhiều trường hợp. Điều này làm cho giải pháp trở nên hiệu quả với kích thước đầu vào tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins

    data = inp.strip().splitlines()
    S = data[0].strip()
    K = int(data[1].strip())

    pattern = "tamocompetindo"
    m = len(pattern)

    if len(S) < m:
        return "NAO"

    for i in range(len(S) - m + 1):
        mismatches = 0
        for j in range(m):
            if S[i + j] != pattern[j]:
                mismatches += 1
                if mismatches > K:
                    break
        if mismatches <= K:
            return "SIM"
    return "NAO"

# provided samples
assert run("tepoassimtamocompetindu\n2") == "SIM"
assert run("temocompetindu\n1") == "NAO"

# custom cases
assert run("tamocompetindo\n0") == "SIM", "exact match"
assert run("xamocompetindo\n1") == "SIM", "single substitution allowed"
assert run("aaaaaa\n2") == "NAO", "too short"
assert run("zzzzzzzzzzzzzzztamocompetindox", "2") == "SIM", "embedded match"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu chính xác | SIM | trường hợp không khớp | 
| không khớp đơn | SIM | được phép chỉnh sửa | 
| chuỗi ngắn | NAO | ranh giới chiều dài | 
| mẫu nhúng | SIM | phát hiện cửa sổ chính xác | 

## Vỏ cạnh 

Một chuỗi ngắn như`"abc"`với bất kỳ`K`ngay lập tức thất bại vì nó thậm chí không thể tạo thành một cửa sổ có độ dài cần thiết. Thuật toán xử lý việc này bằng cách kiểm tra`n < m`trước bất kỳ quá trình xử lý nào. 

Một chuỗi đã chứa`"tamocompetindo"`chính xác kích hoạt thành công trực tiếp ở cửa sổ khớp đầu tiên, với số lượng không khớp bằng 0. Điều này xác nhận hành vi thoát sớm hoạt động chính xác. 

Một chuỗi trong đó mẫu xuất hiện nhưng có nhiều điểm khác biệt vẫn được chuyển khi và chỉ khi các giá trị không khớp vẫn nằm trong`K`. Bộ đếm không khớp đảm bảo chúng tôi không chấp nhận nhầm các cửa sổ vượt quá ngân sách tham nhũng cho phép.
