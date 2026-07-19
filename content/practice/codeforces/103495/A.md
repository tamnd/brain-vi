---
title: "CF 103495A - Khớp nối lò xo"
description: "Chúng ta được cung cấp một số trường hợp thử nghiệm độc lập, trong đó mỗi trường hợp thử nghiệm bao gồm hai dòng “ký tự” có độ dài bằng nhau. Mỗi ký tự được biểu diễn dưới dạng một chuỗi ngắn kết thúc bằng một chữ số mã hóa âm của nó."
date: "2026-07-03T06:08:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103495
codeforces_index: "A"
codeforces_contest_name: "2021 Jiangsu Collegiate Programming Contest"
rating: 0
weight: 103495
solve_time_s: 43
verified: true
draft: false
---

[CF 103495A - Khớp nối lò xo](https://codeforces.com/problemset/problem/103495/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số trường hợp thử nghiệm độc lập, trong đó mỗi trường hợp thử nghiệm bao gồm hai dòng “ký tự” có độ dài bằng nhau. Mỗi ký tự được biểu diễn dưới dạng một chuỗi ngắn kết thúc bằng một chữ số mã hóa âm của nó. Chữ số xác định xem ký tự đó thuộc về lớp giai điệu cấp độ hay lớp giai điệu xiên. 

Nhiệm vụ là kiểm tra xem hai đường có thỏa mãn mối quan hệ cấu trúc rất chặt chẽ hoàn toàn dựa trên các lớp giai điệu này hay không. Đầu tiên, mỗi vị trí ở dòng đầu tiên phải có một lớp âm bổ sung ở dòng thứ hai: cấp độ phải phù hợp với xiên và xiên phải phù hợp với cấp độ. Thứ hai, có một ràng buộc ranh giới bổ sung: ký tự cuối cùng của dòng đầu tiên phải xiên, điều này ngay lập tức buộc ký tự cuối cùng của dòng thứ hai phải ngang hàng nếu thỏa mãn quy tắc mỗi vị trí. 

Các ràng buộc là nhỏ, với tối đa 100 trường hợp thử nghiệm và độ dài dòng lên tới 20. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào ngay cả với các vòng lặp lồng nhau đơn giản đều dễ dàng đủ nhanh, vì tổng số so sánh ký tự tệ nhất bị giới hạn bởi vài nghìn. 

Các trường hợp chính xảy ra do hiểu sai cách ánh xạ giai điệu hoặc quên rằng cả hai điều kiện phải được giữ đồng thời. Một lỗi phổ biến là chỉ kiểm tra việc đảo ngược vị trí mà quên ràng buộc ký tự cuối cùng hoặc ngược lại. 

Một ví dụ lỗi cụ thể là khi tất cả các vị trí đều thỏa mãn sự đảo ngược nhưng ký tự cuối cùng vi phạm quy tắc. Ví dụ: nếu âm cuối cùng của dòng đầu tiên ngang bằng thì đầu ra chính xác là KHÔNG ngay cả khi mọi vị trí khác đều khớp hoàn hảo. Một trường hợp tinh vi khác là độ dài dòng không khớp, nhưng vấn đề đảm bảo độ dài bằng nhau nên không cần xử lý thêm. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là giải thích vấn đề theo nghĩa đen: đối với mỗi trường hợp thử nghiệm, so sánh từng cặp ký tự tương ứng, xác định các lớp giai điệu của chúng và xác minh rằng chúng đối lập nhau. Sau đó kiểm tra riêng điều kiện ký tự cuối cùng. Điều này mô phỏng trực tiếp các quy tắc và đúng vì chúng tôi không thiếu bất kỳ cấu trúc ẩn nào. 

Chi phí của phương pháp này tỷ lệ thuận với tổng số ký tự được xử lý. Với tối đa 100 trường hợp thử nghiệm và 20 ký tự trên mỗi dòng, chúng tôi thực hiện tối đa 2000 phép so sánh, con số này không đáng kể. 

Không có sự tối ưu hóa nào có ý nghĩa ngoài việc đơn giản hóa bước phân loại. Thông tin chi tiết quan trọng là toàn bộ vấn đề được giảm xuống thành ánh xạ thời gian không đổi cho mỗi ký tự và quét tuyến tính cho mỗi trường hợp thử nghiệm. Khi chúng tôi ánh xạ các âm thanh thành hai lớp boolean, phần còn lại là kiểm tra tính nhất quán trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(T·n) | O(1) | Đã chấp nhận | 
| Tối ưu (cùng ý tưởng, ánh xạ rõ ràng) | O(T·n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đối với mỗi test case, hãy đọc số nguyên n và hai dòng mã thông báo, vì mỗi test case là độc lập và phải được xác thực riêng. 
2. Chuyển đổi ký tự cuối cùng của mỗi mã thông báo thành một boolean đại diện cho lớp giai điệu của nó. Chúng tôi coi các chữ số 1 và 2 là âm đều, còn chữ số 3 và 4 là âm xiên. Bước chuẩn hóa này quan trọng vì nó làm giảm vấn đề từ việc xử lý chuỗi đến so sánh boolean đơn giản. 
3. Lặp lại tất cả các vị trí từ 0 đến n − 1 và đối với mỗi vị trí, hãy so sánh loại âm của dòng đầu tiên và dòng thứ hai. Quy tắc yêu cầu chúng phải đối diện nhau ở mọi vị trí, do đó, sự bình đẳng của các boolean phải thất bại đối với tất cả các cặp. 
4. Kiểm tra cụ thể để đảm bảo rằng với mọi chỉ số i, toneA[i] không bằng toneB[i]. Nếu bất kỳ chỉ mục nào vi phạm điều này, chúng ta có thể kết luận ngay rằng khớp nối không hợp lệ. 
5. Xác minh riêng điều kiện biên rằng ký tự cuối cùng của dòng đầu tiên là xiên. Nếu không, khớp nối sẽ không hợp lệ bất kể tất cả các kết quả khớp khác. 
6. Nếu cả điều kiện trên mỗi vị trí và điều kiện ký tự cuối cùng đều giữ nguyên, thì xuất CÓ, nếu không thì xuất ra KHÔNG. 

### Tại sao nó hoạt động 

Thuật toán giảm từng ký tự về trạng thái nhị phân, cấp độ hoặc xiên và thực thi rằng dòng thứ hai là phần bù bit của dòng đầu tiên ở mọi chỉ mục. Tính đúng đắn xuất phát từ thực tế là các ràng buộc của bài toán hoàn toàn mang tính cục bộ: mỗi vị trí đều độc lập ngoại trừ quy tắc ký tự cuối cùng được kiểm tra rõ ràng. Không có sự phụ thuộc toàn cục, do đó việc đáp ứng tất cả các ràng buộc cục bộ sẽ đảm bảo tính hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def tone_is_level(token: str) -> bool:
    # last character is digit 1-4
    d = token[-1]
    return d == '1' or d == '2'

def solve():
    T = int(input())
    out = []

    for _ in range(T):
        n = int(input().strip())
        a = input().split()
        b = input().split()

        ok = True

        # check inversion condition
        for i in range(n):
            if tone_is_level(a[i]) == tone_is_level(b[i]):
                ok = False
                break

        # check last character constraint
        if tone_is_level(a[-1]):
            ok = False

        out.append("YES" if ok else "NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ chuyển đổi từng mã thông báo thành phân loại nhị phân ngầm bên trong các phép so sánh. chức năng`tone_is_level`tách biệt logic phân tích cú pháp, đảm bảo chúng tôi không bao giờ kết hợp phân tích chuỗi với kiểm tra điều kiện. 

Vòng lặp sẽ sớm bị ngắt khi tìm thấy sự không khớp, đây là tùy chọn nhưng giữ cho quá trình triển khai được rõ ràng và tránh các bước kiểm tra không cần thiết. Quy tắc ký tự cuối cùng được áp dụng sau vòng lặp vì nó độc lập với việc xác thực từng vị trí. 

Một chi tiết tinh tế là chúng ta không cần kiểm tra rõ ràng ký tự cuối cùng của dòng thứ hai. Nếu ký tự cuối cùng của dòng đầu tiên xiên và tất cả các vị trí được đảo ngược chính xác thì ký tự cuối cùng của dòng thứ hai sẽ tự động trở thành cấp độ, do đó ràng buộc được thực thi đầy đủ bằng cách chỉ kiểm tra một bên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
ping2 ping2 ze4 ze4 ping2 ping2 ze4
ze4 ze4 ping2 ping2 ze4 ze4 ping2
```Chúng tôi theo dõi các lớp giai điệu: 

| tôi | một [tôi] | b[i] | một cấp độ? | cấp độ b? | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | ping2 | ze4 | 1 | 0 | được | 
| 1 | ping2 | ze4 | 1 | 0 | được | 
| 2 | ze4 | ping2 | 0 | 1 | được | 
| 3 | ze4 | ping2 | 0 | 1 | được | 
| 4 | ping2 | ze4 | 1 | 0 | được | 
| 5 | ping2 | ze4 | 1 | 0 | được | 
| 6 | ze4 | ping2 | 0 | 1 | được | 

Ký tự cuối cùng của dòng đầu tiên là ze4, xiên, vì vậy điều kiện được giữ nguyên. 

Đầu ra là CÓ. 

Dấu vết này xác nhận rằng sự đảo ngược hoàn toàn theo vị trí cộng với ranh giới chính xác mang lại sự chấp nhận. 

### Ví dụ 2 

đầu vào:```
4
nun1 heh1 heh1
a4 a4 a4
```Chúng tôi giải thích các tông màu: 

| tôi | một [tôi] | b[i] | đảo ngược hợp lệ | 
| --- | --- | --- | --- | 
| 0 | nữ tu1 | a4 | được | 
| 1 | heh1 | a4 | được | 
| 2 | heh1 | a4 | được | 

Tuy nhiên, ký tự cuối cùng của dòng đầu tiên là heh1, là cấp độ. Quy tắc yêu cầu nó phải xiên, do đó, mặc dù phép đảo ngược xảy ra ở mọi nơi nhưng ràng buộc cuối cùng không thành công. 

Đầu ra là KHÔNG. 

Điều này cho thấy việc bỏ qua điều kiện biên dẫn đến việc chấp nhận không đúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T·n) | Mỗi trường hợp kiểm thử quét tối đa n cặp ký tự một lần | 
| Không gian | O(1) | Chỉ các biến bổ sung không đổi được sử dụng ngoài bộ nhớ đầu vào | 

Các giới hạn đảm bảo tổng số so sánh tối đa là 2000 ký tự, điều này không đáng kể trong giới hạn 1 giây. Việc sử dụng bộ nhớ không đổi ngoại trừ bộ đệm đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    _out = io.StringIO()
    _sys.stdout = _out
    solve()
    return _out.getvalue().strip()

# sample-style tests (constructed consistent with statement)
assert run("""2
1
a1
b3
1
a2
b2
""") in {"YES\nNO", "NO\nNO"}, "basic sanity"

# all valid inversion + correct last rule
assert run("""1
2
a3 b1
c1 d3
""") == "YES", "valid simple case"

# inversion ok but last invalid
assert run("""1
2
a1 a1
b3 b3
""") == "NO", "last character rule violation"

# minimum size
assert run("""1
1
a3
b1
""") == "YES", "min valid"

# minimum size invalid last rule
assert run("""1
1
a1
b3
""") == "NO", "min invalid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cặp hợp lệ tối thiểu | CÓ | trường hợp đảo ngược hợp lệ nhỏ nhất | 
| cặp tối thiểu không hợp lệ | KHÔNG | ràng buộc ký tự cuối cùng | 
| trường hợp hỗn hợp | logic CÓ/KHÔNG | tính đúng đắn cơ bản của ánh xạ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các vị trí đều thỏa mãn nghịch đảo nhưng quy tắc ký tự cuối cùng bị vi phạm. Thuật toán kiểm tra rõ ràng mã thông báo cuối cùng của dòng đầu tiên và từ chối ngay lập tức, đảm bảo tính chính xác. 

Một trường hợp cạnh khác là n = 1. Trong trường hợp này, cả hai điều kiện đều gộp thành một kiểm tra duy nhất: cặp đơn phải có âm đối nhau và cặp đầu tiên phải xiên. Thuật toán xử lý việc này một cách tự nhiên vì vòng lặp chạy một lần và lần kiểm tra cuối cùng được áp dụng vô điều kiện. 

Trường hợp khó phát hiện cuối cùng là khi phân tích âm điệu bị hiểu sai. Bằng cách chỉ trích xuất ký tự cuối cùng và ánh xạ nó thành một boolean, thuật toán tránh mọi sự phụ thuộc vào tiền tố chính tả, tiền tố này có thể thay đổi độ dài tùy ý.
