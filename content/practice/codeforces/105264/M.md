---
title: "CF 105264M - Kaaa"
description: "Chúng ta được cung cấp một chuỗi ngắn thể hiện những gì Mohanad nghe được lúc 8 giờ sáng. Nhiệm vụ là quyết định xem âm thanh chính xác này có khớp với một mẫu rất cụ thể liên quan đến con quạ hay không: chuỗi phải có chính xác ba lần lặp lại của chuỗi con “Kaaa”, không có ký tự thừa, không…"
date: "2026-06-24T01:32:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105264
codeforces_index: "M"
codeforces_contest_name: "The 2024 Syrian Virtual University Collegiate Programming Contest"
rating: 0
weight: 105264
solve_time_s: 37
verified: true
draft: false
---

[CF 105264M - Kaaa](https://codeforces.com/problemset/problem/105264/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi ngắn thể hiện những gì Mohanad nghe được lúc 8 giờ sáng. Nhiệm vụ là quyết định xem âm thanh chính xác này có khớp với một mẫu rất cụ thể liên quan đến con quạ hay không: chuỗi phải có chính xác ba lần lặp lại của chuỗi con “Kaaa”, không có ký tự thừa, không có chữ cái bị thiếu và không có biến thể về kiểu chữ hoặc khoảng cách. 

Vì vậy, vấn đề về cơ bản là kiểm tra tính bằng nhau của chuỗi một cách nghiêm ngặt đối với chuỗi mục tiêu cố định. Đầu ra là một phân loại: nếu đầu vào khớp chính xác với mẫu, chúng ta nói Mohanad đã thức dậy, nếu không thì anh ta vẫn ngủ. 

Ràng buộc về độ dài chuỗi là nhỏ, tối đa là 100 ký tự. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu nâng cao hoặc các thủ thuật tối ưu hóa. Ngay cả một so sánh trực tiếp hoặc phép biến đổi đơn giản cũng đủ vì các phép toán chuỗi thời gian không đổi hoặc thời gian tuyến tính là không đáng kể ở quy mô này. 

Các trường hợp thất bại chính đều là các dạng gần khớp. Một chuỗi như “KaaaKaaKa” gần nhưng không chính xác vì cấu trúc lặp lại bị hỏng. Một chuỗi như “KaaaKaaaKaaaKaaa” không chính xác vì nó có quá nhiều lần lặp lại. Một chuỗi như “kaaaKaaaKaaa” không chính xác vì chữ hoa chữ thường phải khớp chính xác. Một chuỗi như “KaaaKaaaKaa” không chính xác vì nó thiếu một ký tự ở cuối. 

Điều tinh tế quan trọng nhất là đây không phải là vấn đề “chứa” hay “mẫu xuất hiện bên trong”. Đây là một vấn đề bình đẳng toàn chuỗi không có tính linh hoạt. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là so sánh trực tiếp chuỗi đầu vào với chuỗi mục tiêu “KaaaKaaaKaaa”. Nếu chúng khớp chính xác, chúng tôi xuất ra “Woken Up”, nếu không thì “Still Asleep”. 

Điều này có tác dụng vì sự cố xác định một mẫu cố định duy nhất. Không có sự thay đổi về số lần lặp lại, không có tham số để suy ra và không có yêu cầu khớp từng phần. Thay vào đó, cách diễn giải brute-force sẽ cố gắng xây dựng hoặc xác thực cấu trúc một cách rõ ràng, chẳng hạn bằng cách kiểm tra xem chuỗi có thể được chia thành ba phần bằng nhau hay không và mỗi phần bằng “Kaaa” hay không. Điều đó cũng hiệu quả, nhưng ngay cả điều đó cũng phức tạp không cần thiết do tính chất cố định của mục tiêu. 

Một cách tiếp cận tổng quát hơn sẽ là: xác minh độ dài chính xác là 12, sau đó xác minh s[0:4], s[4:8] và s[8:12] đều bằng “Kaaa”. Điều này có cấu trúc hơn một chút và sẽ khái quát hơn nếu số lần lặp lại thay đổi. Tuy nhiên, đối với vấn đề này, nó chỉ còn một kiểm tra đẳng thức duy nhất. 

Ý tưởng mạnh mẽ về việc kiểm tra lặp đi lặp lại tất cả các phân đoạn có thể có hoặc xây dựng chuỗi con sẽ vẫn chạy trong thời gian không đổi vì đầu vào rất nhỏ nhưng về mặt khái niệm thì đó là chi phí không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra bình đẳng trực tiếp | O(1) | O(1) | Đã chấp nhận | 
| Kiểm tra phân đoạn thủ công | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào s từ đầu vào tiêu chuẩn. 
2. Xác định chuỗi mục tiêu t là “KaaaKaaaKaaa”. 
3. So sánh trực tiếp s với t từng ký tự. 
4. Nếu chúng giống hệt nhau, xuất ra “Woken Up”. 
5. Ngược lại, xuất ra “Still Asleep”. 

### Tại sao nó hoạt động 

Quyết định giảm xuống mức bình đẳng chính xác của chuỗi. Chỉ có một chuỗi hợp lệ thỏa mãn điều kiện, vì vậy điều kiện đúng là nhị phân: mọi ký tự đều khớp với chuỗi dự kiến ​​hoặc ít nhất một vị trí khác nhau. Vì đẳng thức chuỗi kiểm tra ngầm tất cả các vị trí nên không cần lý luận cấu trúc bổ sung. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
target = "KaaaKaaaKaaa"

if s == target:
    print("Woken Up")
else:
    print("Still Asleep")
```Việc triển khai dựa trên tính đẳng thức chuỗi tích hợp của Python, thực hiện so sánh tuyến tính giữa các ký tự. Với độ dài tối đa chỉ là 100, đây thực sự là thời gian không đổi trong thực tế. 

các`.strip()`cuộc gọi đảm bảo chúng tôi loại bỏ ký tự dòng mới ở cuối khỏi đầu vào, điều này rất cần thiết trong cài đặt lập trình cạnh tranh trong đó các dòng đầu vào thô bao gồm`\n`. Nếu không loại bỏ, ngay cả một chuỗi chính xác cũng sẽ không thể so sánh được do có ký tự phụ. 

Không cần phân tích cú pháp hoặc tiền xử lý bổ sung. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`KaaaKaaaKaaa`| Bước | s | mục tiêu | So sánh | 
| --- | --- | --- | --- | 
| 1 | KaaaKaaaKaaa | KaaaKaaaKaaa | Trận đấu | 

Các chuỗi giống hệt nhau theo từng ký tự, vì vậy đầu ra là “Woken Up”. Điều này xác nhận rằng sự lặp lại chính xác của mẫu là hợp lệ. 

Đầu ra:`Woken Up`### Ví dụ 2 

đầu vào:`KaaaKaaKa`| Bước | s | mục tiêu | So sánh | 
| --- | --- | --- | --- | 
| 1 | KaaKaaKa | KaaaKaaaKaaa | Không khớp | 

Sự không phù hợp xảy ra sớm trong lần lặp lại thứ hai. Mặc dù tiền tố khớp nhau nhưng các ký tự bị thiếu sẽ phá vỡ điều kiện đẳng thức đầy đủ. 

Đầu ra:`Still Asleep`Những ví dụ này cho thấy tính đúng từng phần là không đủ; toàn bộ chuỗi phải khớp chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | So sánh đơn trên tối đa 100 ký tự | 
| Không gian | O(1) | Chỉ lưu trữ đầu vào và chuỗi mục tiêu có kích thước cố định | 

Với ràng buộc cực kỳ nhỏ trên n, giải pháp phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. Ngay cả những so sánh lặp đi lặp lại hoặc logic xác thực rõ ràng hơn cũng sẽ không đạt đến bất kỳ ranh giới hiệu suất nào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import sys as _sys
    out = io.StringIO()
    with redirect_stdout(out):
        s = input().strip()
        target = "KaaaKaaaKaaa"
        print("Woken Up" if s == target else "Still Asleep")
    return out.getvalue().strip()

# provided samples
assert run("KaaaKaaaKaaa\n") == "Woken Up"
assert run("KaaaKaaKa\n") == "Still Asleep"

# custom cases
assert run("KaaaKaaaKaaaKaaa\n") == "Still Asleep"   # too long
assert run("kaaaKaaaKaaa\n") == "Still Asleep"       # case mismatch
assert run("KaaaKaaaKaa\n") == "Still Asleep"        # truncated
assert run("KaaaKaaaKaaa\n") == "Woken Up"           # exact match
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| KaaaKaaaKaaa | Thức dậy | đúng mẫu | 
| KaaaKaaaKaaaKaaa | Vẫn Đang Ngủ | quá nhiều lần lặp lại | 
| kaaaKaaaKaaa | Vẫn Đang Ngủ | phân biệt chữ hoa chữ thường | 
| KaaaKaaaKaa | Vẫn Đang Ngủ | ký tự bị thiếu | 

## Vỏ cạnh 

Trường hợp một cạnh là khi đầu vào gần như đúng nhưng có thêm sự lặp lại. Ví dụ: “KaaaKaaaKaaaKaaa” dài hơn dự kiến. Thuật toán so sánh các chuỗi đầy đủ nên hậu tố thừa ngay lập tức gây ra sự không khớp ở cuối, dẫn đến “Still Asleep”. 

Một trường hợp khác là khi cấu trúc bị phá vỡ bên trong một sự lặp lại, chẳng hạn như “KaaaKaaKa”. Mặc dù tiền tố khớp với “Kaaa” đầu tiên nhưng lần lặp lại thứ hai sẽ thất bại sớm. Kiểm tra đẳng thức sẽ phát hiện ký tự khác nhau đầu tiên và loại bỏ chuỗi. 

Trường hợp thứ ba là trường hợp không khớp chữ hoa chữ thường như “kaaaKaaaKaaa”. Quá trình so sánh phân biệt chữ hoa chữ thường, do đó, so sánh ký tự đầu tiên đã không thành công và thuật toán xuất ra “Vẫn đang ngủ” mà không cần phải kiểm tra thêm.
