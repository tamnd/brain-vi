---
title: "CF 102875H - Mã Morse hạnh phúc"
description: "Vấn đề mô tả một mật mã nhỏ giống Morse. Một cuốn sách mật mã chứa nhiều chữ cái, mỗi chữ cái được gán một chuỗi nhị phân duy nhất có độ dài tối đa là năm."
date: "2026-07-25T12:59:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102875
codeforces_index: "H"
codeforces_contest_name: "2020 Jiangsu Collegiate Programming Contest"
rating: 0
weight: 102875
solve_time_s: 42
verified: true
draft: false
---

[CF 102875H - Mã Morse hạnh phúc](https://codeforces.com/problemset/problem/102875/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một mật mã nhỏ giống Morse. Một cuốn sách mật mã chứa nhiều chữ cái, mỗi chữ cái được gán một chuỗi nhị phân duy nhất có độ dài tối đa là năm. Thông báo là một chuỗi nhị phân khác và chúng ta cần xác định có bao nhiêu cách khác nhau để chia thông báo thành các mã chữ cái đã cho. Tùy thuộc vào con số đó, câu trả lời là một trong ba trạng thái: không tồn tại cách giải thích, tồn tại chính xác một cách giải thích hoặc tồn tại nhiều cách giải thích. Trong trường hợp bội số thì số cách dịch phải in theo modulo 128. 

Đầu vào bao gồm một số trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp độ dài của thông báo được mã hóa, số lượng chữ cái có sẵn, từ điển ánh xạ chuỗi ký tự sang chuỗi nhị phân và chuỗi nhị phân cuối cùng cần giải mã. Đầu ra không yêu cầu văn bản được giải mã thực tế. Nó chỉ hỏi về số lượng giải mã có thể. 

Những hạn chế quan trọng định hình giải pháp. Độ dài tin nhắn có thể đạt tới$10^5$, từ điển chứa tối đa 26 mã và mỗi mã có độ dài tối đa là 5. Một giải pháp thử mọi phép phân chia có thể có sẽ tăng theo cấp số nhân vì mọi vị trí có thể hoặc không thể là điểm cắt. Ngay cả một chương trình động so sánh mọi mã ở mọi vị trí cũng chỉ khoảng$26 \times 5 \times n$hoạt động, điều này có thể dễ dàng thực hiện được. 

Phần khó khăn là số câu trả lời chỉ được lấy theo modulo 128 trong trường hợp có nhiều giải pháp. Một lỗi phổ biến là chỉ lưu trữ số modulo 128 và sử dụng nó để quyết định xem có một nghiệm hay không. Ví dụ: nếu số lượng giải mã thực là 128 thì giá trị được lưu trữ là 0, nhưng câu trả lời vẫn là "puppymousecat" chứ không phải "nonono". 

Một trường hợp cạnh khác xuất hiện khi chuỗi có đúng một cách diễn giải. Đối với đầu vào:```
1
1 1
A 0
0
```đầu ra đúng là:```
happymorsecode
```Việc triển khai bất cẩn chỉ kiểm tra xem số đếm có khác 0 hay không sau khi lấy modulo sẽ thất bại trong trường hợp nhiều cách diễn giải giảm xuống 0 modulo 128. 

Trường hợp cạnh thứ hai là một thông báo không thể phân chia được. Đối với đầu vào:```
1
1 1
A 0
1
```đầu ra đúng là:```
nonono
```DP dựa trên quá trình chuyển đổi mà quên khởi tạo tiền tố trống như một cách hợp lệ sẽ báo cáo không chính xác rằng không thể truy cập được trạng thái nào. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là thử đệ quy mọi chữ cái tiếp theo có thể. Ở mọi vị trí, chúng tôi xem xét tất cả các mục từ điển và tiếp tục bất cứ khi nào mã khớp với hậu tố còn lại. Điều này đúng vì mọi giải mã hợp lệ đều tương ứng với chính xác một chuỗi lựa chọn trong phép đệ quy này. Tuy nhiên, số lượng vị trí phân chia có thể có thể là số mũ. Một chuỗi có độ dài$n$có thể có tới$2^{n-1}$nhiều cách khác nhau để đặt dấu phân cách trước khi xem xét từ điển, vì vậy cách tiếp cận này nhanh chóng trở nên bất khả thi. 

Phương pháp vũ phu không thành công vì nó liên tục giải quyết các vấn đề về hậu tố giống nhau. Nếu một vài chữ cái đầu tiên được giải mã theo những cách khác nhau nhưng cả hai đều đạt đến cùng vị trí còn lại thì phần còn lại của tác phẩm giống hệt nhau. Quan sát quan trọng là chỉ có vị trí hiện tại trong chuỗi mới quan trọng. Các lựa chọn trong quá khứ không ảnh hưởng đến các khả năng trong tương lai, do đó bài toán có cấu trúc của một chương trình động tiền tố. 

Chúng tôi để trạng thái đại diện cho tất cả các giải mã của tiền tố. Đối với mọi vị trí có thể truy cập, chúng tôi thử từng từ mã và mở rộng tiền tố nếu các ký tự tiếp theo khớp. Vì các từ mã rất ngắn nên mỗi vị trí chỉ có một lượng công việc không đổi. 

Quan sát thứ hai là về việc đếm. Chúng ta cần hai thông tin: số đếm modulo 128 và số lượng thực có đạt ít nhất hai hay không. Chỉ giữ giá trị modulo sẽ làm mất thông tin vì 128, 256 và nhiều số lớn hơn đều trở thành số 0. Duy trì hai thuộc tính này một cách riêng biệt là đủ để phân biệt ba đầu ra cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(26 * 5 * n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ tất cả các chuỗi mã Morse từ sổ mật mã. Chúng tôi chỉ cần bản thân các mã vì đầu ra phụ thuộc vào số lượng cách diễn giải có thể có chứ không phụ thuộc vào các chữ cái được chọn. 
2. Tạo mảng lập trình động cho tiền tố. Đối với mọi vị trí, hãy lưu trữ số cách để tiếp cận nó theo modulo 128 và một boolean cho biết số cách thực tế để tiếp cận nó có ít nhất là hai hay không. 
3. Khởi tạo tiền tố trống làm một bản giải mã hợp lệ. Chuỗi trống biểu thị điểm bắt đầu trước khi bất kỳ chữ cái nào được chọn. 
4. Xử lý mọi vị trí từ trái sang phải. Nếu có thể truy cập được một vị trí, hãy thử mọi mã trong từ điển. Khi mã khớp với chuỗi con bắt đầu tại vị trí này, hãy thêm các cách của vị trí hiện tại vào vị trí đích. 
5. Khi cập nhật đích, hãy cập nhật số lượng modulo bình thường nhưng cập nhật nhiều cờ một cách độc lập. Cờ nhiều trở thành đúng nếu đích đã có đường đi và có đường đi mới đến hoặc nếu vị trí nguồn đã thể hiện nhiều đường đi. 
6. Sau khi xử lý toàn bộ chuỗi, hãy kiểm tra vị trí cuối cùng. Nếu không có cách nào, hãy in`nonono`. Nếu có đúng một cách thì in`happymorsecode`. Nếu không thì in`puppymousecat`và số cách modulo 128. 

Tại sao nó hoạt động: mỗi lần giải mã tiền tố đều kết thúc ở đúng một vị trí và mỗi chữ cái tiếp theo hợp lệ sẽ tạo ra chính xác một lần chuyển đổi sang tiền tố sau. DP xem xét mọi chuyển đổi có thể xảy ra và chỉ kết hợp các trạng thái thể hiện sự phân chia hợp lệ trước đó. Điều bất biến là sau khi xử lý vị trí$i$, thông tin được lưu trữ cho mỗi tiền tố kết thúc tại$i$thể hiện chính xác tất cả các giải mã có thể có của tiền tố đó. Bởi vì vị trí cuối cùng chứa tất cả các bản giải mã hoàn chỉnh của toàn bộ chuỗi nên trạng thái cuối cùng đưa ra phân loại được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case():
    n, m = map(int, input().split())
    codes = []
    for _ in range(m):
        _, t = input().split()
        codes.append(t)
    s = input().strip()

    mod = [0] * (n + 1)
    many = [False] * (n + 1)

    mod[0] = 1

    def positive(i):
        return mod[i] != 0 or many[i]

    for i in range(n):
        if not positive(i):
            continue
        for c in codes:
            j = i + len(c)
            if j <= n and s.startswith(c, i):
                if positive(j):
                    if positive(i):
                        many[j] = True
                if many[i]:
                    many[j] = True
                mod[j] = (mod[j] + mod[i]) % 128

    if not positive(n):
        return "nonono"
    if not many[n] and mod[n] == 1:
        return "happymorsecode"
    return f"puppymousecat {mod[n]}"

def main():
    t = int(input())
    ans = []
    for _ in range(t):
        ans.append(solve_case())
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Mã giữ hai phần thông tin song song. các`mod`mảng lưu trữ số đếm sau khi áp dụng thao tác modulo cần thiết. các`many`mảng lưu trữ thông tin không thể phục hồi được từ số lượng modulo, cụ thể là liệu số lượng thực đã có ít nhất là hai hay chưa. 

Chức năng trợ giúp`positive`kiểm tra xem một trạng thái có đại diện cho ít nhất một giải mã hay không. Một bang có thể có`mod[i] == 0`nhưng vẫn có thể truy cập được vì số thực của nó có thể là 128 hoặc bội số khác của 128. Sự khác biệt này ngăn cản câu trả lời sai phổ biến nhất. 

Việc chuyển đổi sử dụng`startswith`với độ dài mã ngắn đã biết. Vì mỗi mã có độ dài tối đa là năm nên việc kiểm tra này duy trì thời gian không đổi. Chỉ mục đích được tính toán trước khi kiểm tra ranh giới chuỗi, ngăn chặn các truy cập không hợp lệ. 

Việc khởi tạo`mod[0] = 1`đại diện cho giải mã trống duy nhất trước khi đọc bất kỳ ký tự nào. Không có nó, không có trạng thái nào khác có thể tiếp cận được. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
1
2 2
A 0
B 00
00
```Những thay đổi trạng thái là: 

| Vị trí | Mã phù hợp | Vị trí mới | Cách modulo 128 | Nhiều | 
| --- | --- | --- | --- | --- | 
| 0 | A | 1 | 1 | sai | 
| 0 | B | 2 | 1 | sai | 
| 1 | A | 2 | 2 | đúng | 

Vị trí cuối cùng có hai cách giải thích:`B`Và`AA`. Số lượng modulo là 2 và cờ bội số là đúng, do đó đầu ra là`puppymousecat 2`. 

Ví dụ này chứng tỏ tại sao chỉ đếm các giá trị modulo là không đủ. Nó cũng cho thấy rằng các vị trí phân chia khác nhau thể hiện những cách hiểu khác nhau. 

Coi như:```
1
1 1
A 1
0
```Những thay đổi trạng thái là: 

| Vị trí | Mã phù hợp | Vị trí mới | Cách modulo 128 | Nhiều | 
| --- | --- | --- | --- | --- | 
| 0 | không | không | 1 chỉ khi bắt đầu | sai | 

Vị trí cuối cùng vẫn không thể truy cập được, vì vậy câu trả lời là`nonono`. 

Ví dụ này thực hiện trường hợp ký tự đầu tiên không thể bắt đầu bất kỳ mã Morse nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26 * 5 * n) | Mỗi vị trí kiểm tra tối đa 26 mã và mỗi mã có độ dài tối đa là 5. | 
| Không gian | O(n) | Hai mảng có độ dài n + 1 lưu trữ các trạng thái tiền tố. | 

Tổng độ dài tin nhắn trong tất cả các trường hợp thử nghiệm là$10^5$, do đó phương pháp quy hoạch động tuyến tính dễ dàng nằm trong giới hạn. Thuật toán tránh được các vấn đề về độ sâu đệ quy và không phụ thuộc vào số lượng cách diễn giải có thể có. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    try:
        sys.stdin = io.StringIO(inp)
        out = io.StringIO()
        sys.stdout = out

        t = int(input())
        ans = []

        for _ in range(t):
            n, m = map(int, input().split())
            codes = []
            for _ in range(m):
                _, x = input().split()
                codes.append(x)
            s = input().strip()

            mod = [0] * (n + 1)
            many = [False] * (n + 1)
            mod[0] = 1

            def positive(i):
                return mod[i] != 0 or many[i]

            for i in range(n):
                if not positive(i):
                    continue
                for c in codes:
                    j = i + len(c)
                    if j <= n and s.startswith(c, i):
                        if positive(j) and positive(i):
                            many[j] = True
                        if many[i]:
                            many[j] = True
                        mod[j] = (mod[j] + mod[i]) % 128

            if not positive(n):
                ans.append("nonono")
            elif not many[n] and mod[n] == 1:
                ans.append("happymorsecode")
            else:
                ans.append(f"puppymousecat {mod[n]}")

        return "\n".join(ans)
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run("""1
1 1
A 0
0
""") == "happymorsecode", "single decoding"

assert run("""1
2 2
A 0
B 00
00
""") == "puppymousecat 2", "multiple decoding"

assert run("""1
1 1
A 0
1
""") == "nonono", "impossible decoding"

assert run("""1
8 2
A 0
B 00
00000000
""") == "puppymousecat 34", "larger count"

assert run("""1
3 1
A 1
111
""") == "happymorsecode", "repeated single code"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`với một mã |`happymorsecode`| Giải mã một chữ cái độc đáo | 
|`00`với mã`0`Và`00`|`puppymousecat 2`| Nhiều phần chia hợp lệ | 
| Một chuỗi không có tiền tố phù hợp |`nonono`| Trạng thái cuối cùng không thể truy cập | 
| Tám số không có mã chồng chéo ngắn |`puppymousecat 34`| Đếm nhiều cách hiểu và xử lý modulo | 
| Ba mã có độ dài đơn giống hệt nhau |`happymorsecode`| Một chuỗi chuyển tiếp bắt buộc đơn giản | 

## Vỏ cạnh 

Đối với số đếm là bội số của 128, thuật toán vẫn hoạt động vì`many`cờ lưu trữ thông tin tách biệt với giá trị modulo. Một tin nhắn có thể kết thúc bằng`mod[n] = 0`, nhưng nếu`many[n]`đúng là nó vẫn được phân loại chính xác là có nhiều cách hiểu. 

Đối với một thông báo chỉ có một khả năng phân chia, mọi quá trình chuyển đổi đều đóng góp vào cùng một đường dẫn mà không bao giờ gây ra cách diễn giải thứ hai. Trạng thái cuối cùng được giữ`many[n]`sai và`mod[n]`bằng một, tạo ra`happymorsecode`. 

Đối với thông báo không thể thực hiện được, không có quá trình chuyển đổi nào đạt đến vị trí cuối cùng. Tiền tố trống ban đầu là trạng thái duy nhất có thể truy cập được, vì vậy trạng thái cuối cùng vẫn trống và thuật toán sẽ in`nonono`. 

Đối với các mã chồng chéo như`0`Và`00`, vị trí kết thúc tương tự có thể đạt được từ các vị trí trước đó khác nhau. Quy tắc cập nhật phát hiện rằng đích đến đã có một cách diễn giải khi một cách diễn giải khác đến và đánh dấu nó là nhiều cách diễn giải. Đây là tình huống cần phải theo dõi tính duy nhất riêng biệt.
