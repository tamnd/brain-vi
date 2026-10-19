---
title: "CF 105668B - M(IT)+"
description: "Chúng ta được cấp một chuỗi được xây dựng từ hai loại ký tự, trong đó một loại hoạt động như một điểm đánh dấu cấu trúc và loại còn lại tạo thành một mẫu lặp lại."
date: "2026-06-22T05:12:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105668
codeforces_index: "B"
codeforces_contest_name: "MITIT Winter 2025 Beginner Round"
rating: 0
weight: 105668
solve_time_s: 43
verified: true
draft: false
---

[CF 105668B - M(IT)+](https://codeforces.com/problemset/problem/105668/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi được xây dựng từ hai loại ký tự, trong đó một loại hoạt động như một điểm đánh dấu cấu trúc và loại còn lại tạo thành một mẫu lặp lại. Nhiệm vụ là quyết định xem chuỗi có thể được diễn giải theo một cách rất chặt chẽ hay không: mỗi khi chúng ta gặp ký tự đặc biệt`M`, mọi thứ nối tiếp nó đến cái tiếp theo`M`phải bao gồm một hoặc nhiều bản sao liên tiếp của chuỗi con`IT`. 

Vì vậy, chuỗi được phân vùng một cách hiệu quả bởi`M`. Mỗi đoạn giữa liên tiếp`M`các ký tự, cũng như hậu tố sau ký tự cuối cùng`M`, phải là sự lặp lại không trống của mẫu`IT`. Các ký tự bên ngoài các phân đoạn này không tồn tại trong bài toán này, do đó tính hợp lệ phụ thuộc hoàn toàn vào việc liệu mọi phân đoạn như vậy có thể được phân tách thành`"IT"`lặp đi lặp lại một hoặc nhiều lần mà không có dư lượng. 

Đầu vào là một chuỗi đơn và đầu ra là một quyết định: liệu toàn bộ chuỗi có thỏa mãn ràng buộc cấu trúc này hay không. 

Các ràng buộc không được nêu rõ ràng ở đây, nhưng mô tả giải pháp dự định giả định xử lý tuyến tính trên chuỗi. Điều đó ngụ ý giới hạn thông thường khoảng 10^5 ký tự, điều này ngay lập tức loại trừ mọi cách tiếp cận bậc hai như kiểm tra lại chuỗi con nhiều lần hoặc thử tất cả các phân đoạn. Bất kỳ giải pháp nào cũng phải quét chuỗi một số lần không đổi. 

Một trường hợp thất bại tinh tế xuất phát từ việc bỏ qua sự liên kết. Ví dụ, hãy xem xét`"MITITMII"`. Phân khúc`"ITIT"`là hợp lệ, nhưng`"II"`thì không, mặc dù cả hai chữ cái đều xuất hiện theo cặp ở nơi khác. Một cách tiếp cận ngây thơ chỉ kiểm tra số lượng hoặc chỉ kiểm tra sự hiện diện của`I`Và`T`sẽ chấp nhận không chính xác các phân đoạn không hợp lệ. Một trường hợp thất bại khác phát sinh nếu chúng ta không thực thi sự lặp lại đó là liền kề:`"ITTI"`chứa nhiều bộ ký tự phù hợp nhưng không bao gồm`"IT"`khối. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ xử lý từng`M`làm ranh giới và xác nhận phân khúc tiếp theo bằng cách cố gắng tiêu thụ`"IT"`ghép nối một cách tham lam hoặc bằng cách tính toán lại các kết quả chuỗi con lặp đi lặp lại. Trong trường hợp xấu nhất, nếu chuỗi bao gồm chủ yếu là`I`Và`T`giữa thưa thớt`M`s, chúng tôi có thể liên tục quét cùng một ký tự nhiều lần, dẫn đến hành vi bậc hai. 

Quan sát quan trọng là mỗi phân đoạn là độc lập và mỗi nhân vật trong một phân đoạn tham gia vào chính xác một tiềm năng.`"IT"`đôi. Điều này gợi ý rằng thay vì liên tục kiểm tra các chuỗi con, chúng ta có thể xử lý từng phân đoạn một lần và xác thực nó trong một lần duy nhất. Ngay khi chúng tôi rời khỏi`M`, chúng ta biết chúng ta phải đọc một chuỗi`"ITITIT..."`cho đến lần tiếp theo`M`hoặc kết thúc, vì vậy chúng ta chỉ cần xác minh cấu trúc này một cách tham lam bằng cách kiểm tra các ký tự theo cặp. 

Điều này làm giảm vấn đề thành quét tuyến tính với trạng thái đơn giản: chúng tôi mong đợi`I`, sau đó`T`, lặp đi lặp lại và bất kỳ sai lệch nào sẽ ngay lập tức làm mất hiệu lực của chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi từ trái sang phải trong khi tôn trọng ranh giới phân đoạn được tạo bởi`M`. 

1. Bắt đầu quét từ đầu chuỗi. Chúng tôi đối xử với nhau`M`làm điểm đặt lại bắt đầu khối xác thực mới. Điều này quan trọng vì mỗi khối phải thỏa mãn quy tắc lặp lại một cách độc lập. 
2. Khi chúng ta gặp phải một`M`, chúng ta chuyển sang ký tự tiếp theo và bắt đầu xác thực một phân đoạn phải bao gồm toàn bộ`"IT"`sự lặp lại. Yêu cầu bắt đầu ngay sau đó`M`đảm bảo chúng tôi không cho phép tiền tố trống hoặc không đúng định dạng. 
3. Bên trong một đoạn, chúng ta kiểm tra các ký tự theo cặp. Ký tự đầu tiên của một cặp hợp lệ phải là`I`. Nếu không, chuỗi đó sẽ không hợp lệ ngay lập tức vì không được phép sắp xếp lại. 
4. Sau khi đọc một`I`, chúng ta phải thấy một`T`ngay sau đó. Nếu ký tự tiếp theo bị thiếu hay không`T`, chúng tôi từ chối chuỗi. Điều này thực thi cấu trúc cố định của`"IT"`như một khối nguyên tử. 
5. Chúng ta tiếp tục tiêu thụ`"IT"`cặp cho đến khi chúng tôi gặp nhau`M`hoặc đến cuối chuỗi. Nếu chúng ta dừng chính xác ở một ranh giới thì đoạn đó hợp lệ; mặt khác, bất kỳ ký tự còn sót lại nào bên trong một phân đoạn đều biểu thị một cặp không đầy đủ và không hợp lệ. 
6. Lặp lại quy trình này cho tất cả các phân đoạn được phân tách bằng`M`. Nếu tất cả các phân đoạn đều vượt qua xác thực thì chuỗi đó hợp lệ. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến rằng bất cứ khi nào chúng ta ở trong một phân đoạn, số lượng ký tự được sử dụng kể từ phân đoạn cuối cùng`M`luôn luôn chẵn và đã được xác minh là hợp lệ`"IT"`cặp. Mỗi bước mở rộng bất biến này thêm đúng hai ký tự. Bởi vì mỗi ký tự được sử dụng chính xác một lần và các phân đoạn được phân tách chặt chẽ bằng`M`, không thể tồn tại sự phụ thuộc giữa các phân đoạn. Do đó, mọi vi phạm phải xuất hiện cục bộ dưới dạng không khớp trong một cặp mà thuật toán sẽ phát hiện ngay lập tức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
n = len(s)

i = 0
while i < n:
    if s[i] == 'M':
        i += 1
        while i < n and s[i] != 'M':
            if i + 1 >= n:
                print("NO")
                sys.exit(0)
            if s[i] != 'I' or s[i + 1] != 'T':
                print("NO")
                sys.exit(0)
            i += 2
    else:
        print("NO")
        sys.exit(0)

print("YES")
```Mã này phản ánh trực tiếp logic dựa trên phân đoạn. Vòng lặp bên ngoài di chuyển qua chuỗi và mỗi khi nó nhìn thấy`M`, nó sẽ chuyển sang chế độ xác thực cho một phân đoạn. Bên trong đoạn đó, con trỏ tiến lên theo từng bước hai, thực thi nghiêm ngặt`"IT"`ghép nối. Việc kiểm tra giới hạn đảm bảo chúng tôi không chấp nhận các cặp không đầy đủ ở cuối phân đoạn. 

Một cạm bẫy triển khai phổ biến là quên thực thi các phân đoạn bắt đầu ngay sau`M`. điều kiện`else: print("NO")`đảm bảo rằng bất kỳ ký tự nào nằm ngoài ngữ cảnh phân đoạn hợp lệ sẽ làm mất hiệu lực chuỗi. Một vấn đề tế nhị khác là quên kiểm tra`i + 1 >= n`, điều này ngăn cản việc đọc vượt quá chuỗi khi một`I`xuất hiện không khớp`T`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`"MITITMITIT"`| tôi | char | hành động | trạng thái phân đoạn | 
| --- | --- | --- | --- | 
| 0 | M | đoạn bắt đầu | đặt lại | 
| 1 | Tôi | mong đợi cặp | bắt đầu CNTT | 
| 2 | T | cặp hợp lệ | được | 
| 3 | Tôi | cặp tiếp theo | bắt đầu CNTT | 
| 4 | T | cặp hợp lệ | được | 
| 5 | M | đóng đoạn | hợp lệ | 
| 6 | Tôi | phân khúc mới | bắt đầu CNTT | 
| 7 | T | cặp hợp lệ | được | 
| 8 | Tôi | cặp tiếp theo | bắt đầu CNTT | 
| 9 | T | cặp hợp lệ | được | 

Điều này thể hiện một chuỗi hoàn toàn hợp lệ trong đó mỗi phân đoạn là một sự kết nối rõ ràng của`"IT"`cặp. 

### Ví dụ 2 

đầu vào:`"MITTMITIT"`| tôi | char | hành động | trạng thái phân đoạn | 
| --- | --- | --- | --- | 
| 0 | M | đoạn bắt đầu | đặt lại | 
| 1 | Tôi | mong đợi cặp | bắt đầu CNTT | 
| 2 | T | cặp hợp lệ | được | 
| 3 | T | không hợp lệ | không khớp | 

Ở đây phân đoạn đầu tiên thất bại vì`"TT"`phá vỡ yêu cầu`"IT"`cấu trúc mặc dù tổng số ký tự là số chẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được truy cập nhiều nhất một lần trong quá trình quét cặp | 
| Không gian | O(1) | Chỉ sử dụng con trỏ và không sử dụng cấu trúc phụ trợ | 

Giải pháp tuyến tính theo chiều dài của chuỗi, vừa vặn thoải mái trong các ràng buộc thông thường đối với các chuỗi lên tới 10^5 trở lên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    backup = sys.stdout
    sys.stdout = io.StringIO()

    # inline solution
    s = sys.stdin.readline().strip()
    n = len(s)

    i = 0
    ok = True
    while i < n:
        if s[i] == 'M':
            i += 1
            while i < n and s[i] != 'M':
                if i + 1 >= n:
                    ok = False
                    break
                if s[i] != 'I' or s[i + 1] != 'T':
                    ok = False
                    break
                i += 2
            if not ok:
                break
        else:
            ok = False
            break

    print("YES" if ok else "NO")

    out = sys.stdout.getvalue().strip()
    sys.stdout = backup
    return out

# provided-like samples
assert run("MITITMITIT") == "YES"
assert run("MITTMITIT") == "NO"

# custom cases
assert run("M") == "YES", "empty valid segment"
assert run("MITIT") == "YES", "single segment"
assert run("MITI") == "NO", "odd leftover"
assert run("TMITIT") == "NO", "invalid start"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| M | CÓ | cạnh đoạn trống | 
| MITIT | CÓ | khối hợp lệ duy nhất | 
| MITI | KHÔNG | cặp không đầy đủ | 
| TMITIT | KHÔNG | tiền tố không hợp lệ trước M | 

## Vỏ cạnh 

Trường hợp một cạnh là một chuỗi bao gồm một`M`. Thuật toán ngay lập tức nhập một đoạn và không tìm thấy đoạn nào không hợp lệ`"IT"`cặp, do đó nó chấp nhận nó là hợp lệ vì không vi phạm quy tắc lặp lại. 

Một trường hợp cạnh khác là đầu vào bắt đầu bằng`I`hoặc`T`trước bất kỳ`M`. Ví dụ`"ITM..."`hoặc`"TM..."`bị từ chối ngay lập tức vì mọi cấu trúc hợp lệ chỉ phải bắt đầu một phân đoạn sau`M`và không có độc lập`"IT"`được phép bên ngoài bối cảnh đó. 

Trường hợp cạnh thứ ba là một đoạn kết thúc bằng một ký tự còn sót lại. Vì`"MITI"`, sau khi tiêu thụ`"IT"`, cuối cùng`I`không có đối tác`T`, và kiểm tra ranh giới nắm bắt được điều này bởi vì`i + 1`vượt quá giới hạn phân đoạn, tạo ra sự từ chối.
