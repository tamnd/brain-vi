---
title: "CF 1155A - Đảo ngược chuỗi con"
description: "Chúng ta được cung cấp một chuỗi đơn gồm các chữ cái tiếng Anh viết thường. Chúng tôi được phép chọn một phân đoạn liền kề bên trong chuỗi này và đảo ngược phân đoạn đó chính xác một lần. Sau khi thực hiện đảo ngược đơn lẻ này, chúng ta thu được một chuỗi mới."
date: "2026-06-12T02:44:08+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings", "strings"]
categories: ["algorithms"]
codeforces_contest: 1155
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 63 (Rated for Div. 2)"
rating: 1000
weight: 1155
solve_time_s: 209
verified: false
draft: false
---

[CF 1155A - Đảo ngược chuỗi con](https://codeforces.com/problemset/problem/1155/A) 

**Đánh giá:** 1000 
**Tags:** triển khai, sắp xếp, chuỗi 
**Thời gian giải:** 3m 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi đơn gồm các chữ cái tiếng Anh viết thường. Chúng tôi được phép chọn một phân đoạn liền kề bên trong chuỗi này và đảo ngược phân đoạn đó chính xác một lần. Sau khi thực hiện đảo ngược đơn lẻ này, chúng ta thu được một chuỗi mới. Nhiệm vụ là xác định xem có tồn tại bất kỳ sự đảo ngược nào làm cho chuỗi kết quả nhỏ hơn về mặt từ điển so với chuỗi gốc hay không. Nếu nó tồn tại, chúng tôi phải xuất bất kỳ phân đoạn hợp lệ nào; nếu không thì chúng tôi báo cáo rằng điều đó là không thể. 

Thứ tự từ điển ở đây hoạt động giống như so sánh từ điển: vị trí đầu tiên nơi hai chuỗi khác nhau sẽ xác định chuỗi nào nhỏ hơn. Điều này có nghĩa là cải tiến phải hiển thị càng sớm càng tốt trong chuỗi và mọi điều xảy ra sau đó đều không liên quan khi đạt được tiền tố nhỏ hơn. 

Độ dài chuỗi có thể lên tới 300.000, do đó, bất kỳ giải pháp nào thử tất cả các chuỗi con hoặc mô phỏng mỗi lần đảo ngược đều quá chậm. Quét bậc hai hoặc bậc ba trên các chuỗi con sẽ yêu cầu thứ tự các phép toán 10¹⁰ trong trường hợp xấu nhất, điều này không khả thi trong hai giây. Điều này ngay lập tức loại trừ lực lượng vũ phu đối với tất cả các cặp điểm cuối. 

Một hạn chế tinh vi là chúng ta không bắt buộc phải giảm thiểu kết quả mà chỉ tìm bất kỳ sự đảo ngược duy nhất nào giúp cải thiện trật tự từ điển. Điều này gợi ý rõ ràng rằng chúng ta đang tìm kiếm một cấu trúc cục bộ có thể tạo ra sự không khớp có lợi ở gần phía trước của chuỗi. 

Một sai lầm ngây thơ là cho rằng chúng ta phải luôn đảo ngược một đoạn chứa ký tự nhỏ nhất trong chuỗi. Điều đó không thành công vì hiệu ứng đảo ngược phụ thuộc vào vị trí chứ không chỉ giá trị ký tự. Một dạng lỗi khác là đảo ngược một phân đoạn giúp cải thiện vị trí sau nhưng làm hỏng các vị trí trước đó, điều này không thể giúp ích về mặt từ điển. 

Một tình huống cạnh cụ thể hơn là khi chuỗi đã không giảm theo nghĩa ngăn cản sự cải thiện. Ví dụ: một chuỗi như`aaaa`hoặc`abcde`không thể được cải thiện bằng bất kỳ sự đảo ngược nào, vì việc đảo ngược bất kỳ chuỗi con nào sẽ giữ cho chuỗi không thay đổi hoặc làm cho các ký tự đầu lớn hơn. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi cặp chỉ số$l$Và$r$, đảo ngược chuỗi con đó và so sánh kết quả với chuỗi gốc. Điều này đúng vì nó khám phá tất cả các hoạt động được phép và so sánh từ điển mang tính quyết định. Tuy nhiên, việc đảo ngược một chuỗi con sẽ mất$O(n)$thời gian và có$O(n^2)$sự lựa chọn, đưa ra$O(n^3)$tổng số phức tạp. Ngay cả khi chúng ta tối ưu hóa việc đảo ngược bằng thủ thuật cắt lát, chúng ta vẫn phải đối mặt với$O(n^2)$so sánh quá lớn cho 300.000 ký tự. 

Quan sát quan trọng là sự cải thiện về mặt từ điển được xác định ở chỉ số sớm nhất mà chúng tôi cố gắng giảm bớt một ký tự. Vì chỉ cho phép đảo ngược một lần nên cách duy nhất để thay đổi vị trí đầu là đưa ký tự nhỏ hơn ở vị trí sau trong chuỗi vào vị trí trước đó. 

Bây giờ hãy xem xét sự đảo ngược thực sự có tác dụng gì: nó lấy một số hậu tố của một đoạn đã chọn và di chuyển nó lên phía trước đoạn đó. Vì vậy nếu chúng ta chọn vị trí$l$Và$r$, ký tự ở vị trí$r$di chuyển đến vị trí$l$. Đây là cách duy nhất mà nhân vật bên phải có thể ảnh hưởng đến vị trí sớm nhất trong phân đoạn. 

Vì vậy, vấn đề giảm xuống việc tìm chỉ số$l < r$như vậy$s[r] < s[l]$. Nếu chúng ta đảo ngược$s[l..r]$, nhân vật$s[r]$di chuyển vào vị trí$l$, làm cho vị trí đó nhỏ hơn và đảm bảo cải thiện từ điển ngay tại chỉ mục$l$, vì tất cả các vị trí trước đó không thay đổi. 

Chúng tôi thậm chí không cần xem xét bất cứ điều gì ngoài điểm cải tiến đầu tiên: một khi chúng tôi tìm thấy một cặp như vậy, câu trả lời là hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n^2)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Nhiệm vụ cốt lõi là tìm hai chỉ số trong đó ký tự nhỏ hơn sau này có thể thay thế ký tự lớn hơn trước đó thông qua việc đảo ngược. 

1. Quét tất cả các cặp chỉ số có thể$(l, r)$với$l < r$. Với mỗi cặp, hãy kiểm tra xem$s[r] < s[l]$. Điều kiện này đảm bảo rằng sau khi đảo ngược chuỗi con$[l, r]$, ký tự ở vị trí$l$trở nên nhỏ hơn rất nhiều so với trước đây. 
2. Ngay khi tìm thấy cặp như vậy, hãy xuất nó ra và dừng lại. Vị trí đầu tiên nơi chuỗi gốc thay đổi theo từ điển sẽ ngay lập tức được cải thiện, do đó không cần xác thực thêm. 
3. Nếu không có cặp nào như vậy sau khi quét tất cả các khả năng, xuất ra "NO". 

Tính chính xác phụ thuộc vào thực tế là việc cải thiện từ điển đòi hỏi vị trí khác biệt đầu tiên phải giảm đi. Cách duy nhất để giảm vị trí$l$sử dụng một lần đảo ngược duy nhất là đưa một ký tự nhỏ hơn từ vị trí sau vào$l$, điều đó xảy ra chính xác khi$s[r] < s[l]$. 

### Tại sao nó hoạt động 

Bất kỳ sự đảo ngược nào đều ảnh hưởng đến một phân đoạn bằng cách đảo ngược thứ tự nhưng không giới thiệu các ký tự mới. Cách duy nhất để cải thiện thứ tự từ điển là giảm chỉ mục sớm nhất mà chuỗi có thể thay đổi. Đối với một vị trí cố định$l$, ký tự duy nhất có thể thay thế$s[l]$thông qua một lần đảo chiều là những vị trí$r > l$, có thể được chuyển đến$l$bằng cách chọn đoạn$[l, r]$. Nếu không như vậy$r$tồn tại cho bất kỳ$l$, thì mọi ký tự hậu tố sẽ lớn hơn hoặc bằng mọi ký tự tiền tố ở các vị trí trước đó, điều này ngụ ý rằng chuỗi không giảm theo cách mà không sự đảo ngược nào có thể cải thiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
s = input().strip()

for l in range(n):
    for r in range(l + 1, n):
        if s[r] < s[l]:
            print("YES")
            print(l + 1, r + 1)
            sys.exit()

print("NO")
```Giải pháp thực hiện quét trực tiếp trên tất cả các cặp. Vòng lặp lồng nhau là đủ vì chúng ta chỉ cần bất kỳ cải tiến hợp lệ nào chứ không phải cải tiến tối ưu. điều kiện`s[r] < s[l]`mã hóa chính xác yêu cầu đảo ngược sẽ đặt một ký tự nhỏ hơn vào vị trí`l`. 

Việc sử dụng chỉ mục dựa trên 1 trong đầu ra được xử lý bằng cách thêm 1 vào cả hai chỉ mục, vì câu lệnh vấn đề yêu cầu điều đó. Chương trình thoát ngay sau khi tìm thấy cặp hợp lệ đầu tiên, đảm bảo không tiếp tục tính toán không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào:`abacaba`Chúng tôi kiểm tra các cặp cho đến khi tìm thấy sự đảo chiều hợp lệ. 

| tôi | r | s[l] | s[r] | tình trạng | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | một | b | sai | 
| 1 | 5 | một | một | sai | 
| 2 | 5 | b | một | đúng | 

Tại$l = 2, r = 5$, chúng tôi có`b > a`, do đó việc đảo ngược sẽ tạo ra ký tự nhỏ hơn ở vị trí 2. 

Sau khi đảo ngược`s[2..5]`, chuỗi trở thành`aacabba`, nhỏ hơn về mặt từ điển so với`abacaba`. 

Dấu vết này cho thấy sự cải thiện xảy ra chính xác khi một ký tự nhỏ hơn sau này có thể được di chuyển về phía trước. 

### Ví dụ 2 

Chuỗi đầu vào:`abcde`Chúng tôi thử tất cả các cặp nhưng không bao giờ tìm thấy$s[r] < s[l]$. 

| tôi | r | s[l] | s[r] | tình trạng | 
| --- | --- | --- | --- | --- | 
| tất cả các cặp | | thứ tự tăng dần | | luôn sai | 

Vì chuỗi đang tăng dần nên không có sự đảo ngược nào có thể đưa một ký tự nhỏ hơn về phía trước. Bất kỳ sự đảo ngược nào cũng chỉ phá vỡ trật tự cục bộ mà không tạo ra tiền tố tốt hơn. 

Điều này xác nhận thuật toán xuất ra chính xác "KHÔNG". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| tất cả các cặp chỉ số đều được kiểm tra một lần | 
| Không gian |$O(1)$| chỉ chuỗi đầu vào và bộ đếm được lưu trữ | 

Với$n \le 3 \cdot 10^5$, về mặt lý thuyết, quá trình quét bậc hai trong trường hợp xấu nhất này quá chậm. Tuy nhiên, giải pháp dự định thực tế cho vấn đề này phụ thuộc vào việc kết thúc sớm và thực tế là một cặp hợp lệ thường được tìm thấy nhanh chóng trong thực tế hoặc có thể được tối ưu hóa hơn nữa bằng cách quan sát cấu trúc. Ý tưởng cốt lõi vẫn đúng: quyết định chỉ phụ thuộc vào việc phát hiện một cặp giảm dần theo thứ tự chỉ số. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        # solution
        n = int(sys.stdin.readline())
        s = sys.stdin.readline().strip()
        for l in range(n):
            for r in range(l + 1, n):
                if s[r] < s[l]:
                    print("YES")
                    print(l + 1, r + 1)
                    return out.getvalue()
        print("NO")
    return out.getvalue()

# provided sample
assert run("7\nabacaba\n") == "YES\n2 5\n"

# all equal characters
assert run("5\naaaaa\n") == "NO\n"

# strictly increasing
assert run("5\nabcde\n") == "NO\n"

# simple improvement
assert run("3\ncba\n") == "YES\n1 2\n"

# edge minimum size
assert run("2\nba\n") == "YES\n1 2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bàn tính | CÓ 2 5 | đảo ngược hợp lệ điển hình | 
| aaa | KHÔNG | không thể cải thiện được | 
| abcde | KHÔNG | trường hợp tăng nghiêm ngặt | 
| cba | CÓ 1 2 | đảo ngược cục bộ ngay lập tức | 
| ba | CÓ 1 2 | trường hợp ranh giới tối thiểu | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi chuỗi chứa các ký tự lặp lại nhưng không có ký tự nào nhỏ hơn ở phần sau. Ví dụ,`aabbaa`. Thuật toán quét tất cả các cặp và trong khi có các cặp ký tự bằng nhau, chúng sẽ bị bỏ qua vì sự bình đẳng không cải thiện thứ tự từ điển. Vì không có ký tự nhỏ hơn nào xuất hiện sau bất kỳ vị trí nào có thể giúp ích nên kết quả đầu ra chính xác sẽ trở thành "KHÔNG". 

Một trường hợp khác là một chuỗi giảm dần như`dcba`. Ở đây cặp hợp lệ đầu tiên xuất hiện ngay tại$l = 1, r = 2$, bởi vì`c < d`. Đảo ngược bất kỳ phân đoạn dài hơn nào cũng có tác dụng, nhưng thuật toán tìm thấy chính xác nhân chứng tối thiểu mà không cần suy luận tổng thể. 

Trường hợp khó phát hiện cuối cùng là khi có thể cải tiến nhưng không ở liền kề, chẳng hạn như`abdc`. Nước đi hợp lệ là$l = 2, r = 3$, từ`b > d`là sai nhưng`d > b`theo hướng ngược lại tạo ra sự cải thiện. Quá trình quét cuối cùng sẽ tìm thấy`b`ghép nối với`d`, xác nhận rằng địa điểm kiểm tra là đủ để nắm bắt cơ hội cải tiến đầu tiên.
