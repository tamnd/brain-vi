---
title: "CF 1533D - Tìm kiếm chuỗi"
description: "Chúng ta được cung cấp một tập hợp các chuỗi cố định, tất cả đều có cùng độ dài và chúng ta được yêu cầu trả lời nhiều truy vấn về một chuỗi dài hơn một chút. Mỗi chuỗi được lưu trữ có độ dài $m$. Mỗi chuỗi truy vấn có độ dài $m+1$."
date: "2026-06-14T18:34:48+07:00"
tags: ["codeforces", "competitive-programming", "*special", "hashing"]
categories: ["algorithms"]
codeforces_contest: 1533
codeforces_index: "D"
codeforces_contest_name: "Kotlin Heroes: Episode 7"
rating: 0
weight: 1533
solve_time_s: 211
verified: true
draft: false
---

[CF 1533D - Tìm kiếm chuỗi](https://codeforces.com/problemset/problem/1533/D) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt, băm 
**Thời gian giải:** 3 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các chuỗi cố định, tất cả đều có cùng độ dài và chúng ta được yêu cầu trả lời nhiều truy vấn về một chuỗi dài hơn một chút. 

Mỗi chuỗi được lưu trữ có độ dài$m$. Mỗi chuỗi truy vấn có độ dài$m+1$. Đối với một chuỗi truy vấn$t$, chúng tôi muốn đếm xem có bao nhiêu chuỗi ban đầu$s_i$có thể biến thành$t$bằng cách chèn chính xác một ký tự vào đâu đó trong$s_i$. Ký tự được chèn có thể ở bất kỳ vị trí nào, vì vậy chúng tôi đang kiểm tra một cách hiệu quả xem việc xóa một ký tự khỏi$t$có thể sản xuất$s_i$. 

Vì vậy, mỗi truy vấn sẽ hỏi: trong số tất cả các chuỗi từ điển có độ dài$m$, có bao nhiêu bằng một số độ dài-$m$dãy con thu được bằng cách xóa chính xác một ký tự khỏi chuỗi truy vấn. 

Những hạn chế ngay lập tức định hình giải pháp. Số lượng chuỗi và truy vấn đều có thể đạt tới$10^5$, trong khi độ dài của mỗi chuỗi tối đa là 10. Chi tiết cuối cùng đó mang tính quyết định: mặc dù kích thước đầu vào lớn nhưng các chuỗi lại cực kỳ ngắn. Điều này gợi ý rằng bất kỳ giải pháp nào cũng nên xử lý trước tất cả các chuỗi theo cách cho phép thời gian truy vấn không đổi hoặc gần như không đổi và sự phụ thuộc theo cấp số nhân vào$m$có thể chấp nhận được vì$m \le 10$. 

Một cách tiếp cận đơn giản sẽ so sánh từng truy vấn với mọi chuỗi được lưu trữ và thử xóa từng vị trí có thể. Cho mỗi cặp$(s_i, t)$, chúng tôi sẽ kiểm tra xem có xóa một ký tự khỏi$t$sản lượng$s_i$. Chi phí đó$O(m)$mỗi lần kiểm tra, và có$nq = 10^{10}$cặp trong trường hợp xấu nhất là quá lớn. 

Một vấn đề tinh tế hơn sẽ xuất hiện nếu chúng ta cố gắng “chuẩn hóa” chuỗi không chính xác. Ví dụ: nếu chúng ta tính toán trước các giá trị băm của chuỗi nhưng quên rằng vị trí xóa thay đổi, thì chúng ta có thể gộp các phần nhúng khác nhau. Một cạm bẫy khác là giả sử chúng ta chỉ có thể khớp tiền tố hoặc hậu tố, nhưng ký tự được chèn có thể xuất hiện ở bất cứ đâu, do đó việc phân tách nghiêm ngặt chỉ bằng tiền tố/hậu tố là không đủ trừ khi chúng ta liệt kê vị trí phân tách. 

Các trường hợp cạnh rất đơn giản nhưng đáng được nói rõ. Nếu tất cả các chuỗi giống hệt nhau thì mọi truy vấn sẽ tính tất cả các kết quả trùng khớp tùy thuộc vào số lượng vị trí xóa khớp với chuỗi đó. Nếu truy vấn chứa các chữ cái lặp lại, nhiều vị trí xóa có thể tạo ra các chuỗi kết quả giống hệt nhau và việc đếm phải tránh tính hai lần thông qua bản đồ tần số tập hợp hoặc hàm băm. 

## Phương pháp tiếp cận 

Giải pháp brute Force lặp lại trên mọi chuỗi và đối với mỗi truy vấn, thử xóa từng vị trí khỏi chuỗi truy vấn để tạo thành chuỗi ứng cử viên, sau đó kiểm tra xem ứng cử viên đó có tồn tại trong một tập hợp chuỗi đầu vào hay không. Vì mỗi truy vấn có$m+1$có thể xóa và mỗi lần kiểm tra là$O(1)$trung bình bằng cách sử dụng bộ băm, điều này làm giảm chi phí truy vấn xuống$O(m)$. Tuy nhiên, điều này bỏ qua tính đa dạng của các cách khác nhau để khớp một chuỗi được lưu trữ và quan trọng hơn là nó vẫn yêu cầu tính toán cẩn thận trên mỗi chuỗi, điều này sẽ không hiệu quả nếu được triển khai một cách đơn giản trên mỗi cặp. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì hỏi liệu mỗi chuỗi được lưu trữ có thể khớp với một truy vấn hay không, chúng tôi tính toán trước tất cả “siêu chuỗi lân cận” của các chuỗi được lưu trữ. Đối với mỗi chuỗi được lưu trữ$s$, chúng tôi xem xét tất cả các cách có thể để chèn một ký tự vào đó. Điều đó tạo ra một tập hợp độ dài$m+1$chuỗi, nhưng việc liệt kê rõ ràng tất cả 26 lựa chọn cho mỗi vị trí sẽ là quá lớn. 

Thay vào đó, chúng tôi quan sát thấy tính đối xứng rõ ràng hơn: một truy vấn khớp với một chuỗi được lưu trữ khi và chỉ khi loại bỏ chính xác một vị trí khỏi truy vấn sẽ mang lại chuỗi được lưu trữ. Do đó, chúng ta có thể xử lý trước tất cả các chuỗi được lưu trữ trong tập băm. Sau đó, với mỗi truy vấn, chúng tôi tạo ra tất cả$m+1$các ứng cử viên thu được bằng cách xóa một ký tự và đếm xem có bao nhiêu ứng cử viên này tồn tại trong tập hợp. 

Từ$m \le 10$, mỗi truy vấn chỉ yêu cầu tối đa 11 lần kiểm tra chuỗi con, tức là thời gian không đổi. Chúng tôi chỉ cần đảm bảo các bản sao không được tính hai lần khi các lần xóa khác nhau tạo ra cùng một chuỗi kết quả (điều này xảy ra khi truy vấn có các ký tự lặp lại). Một tập hợp cục bộ cho mỗi truy vấn sẽ giải quyết được điều này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu theo cặp |$O(nqm)$|$O(n)$| Quá chậm | 
| Bảng liệt kê xóa + Bộ băm |$O(nm + qm)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các chuỗi được lưu trữ và chèn chúng vào bộ băm. 

Điều này cho phép kiểm tra tư cách thành viên liên tục khi so sánh với các ứng cử viên được tạo. 
2. Đối với mỗi chuỗi truy vấn$t$, khởi tạo một tập hợp cục bộ trống để theo dõi kết quả ứng cử viên nào đã được tính. 

Điều này ngăn cản việc đếm hai lần khi hai vị trí xóa khác nhau tạo ra cùng một chuỗi. 
3. Lặp lại mọi chỉ mục$i$trong chuỗi truy vấn từ$0$ĐẾN$m$. 

Đối với mỗi chỉ mục, hãy xây dựng một chuỗi ứng cử viên bằng cách loại bỏ$t[i]$. 

Điều này tương ứng với việc kiểm tra nghịch đảo của hoạt động được phép. 
4. Nếu chuỗi ứng cử viên này tồn tại trong tập hợp được lưu trữ và chưa được tính cho truy vấn này, hãy tăng câu trả lời và đánh dấu nó là đã thấy. 
5. Xuất số liệu cuối cùng cho truy vấn. 

### Tại sao nó hoạt động 

Mỗi chuỗi được lưu trữ$s_i$có thể xuất hiện trong câu trả lời cho một truy vấn$t$nếu và chỉ khi tồn tại một chỉ mục trong$t$loại bỏ của nó mang lại chính xác$s_i$. Thuật toán liệt kê rõ ràng tất cả các khả năng như vậy. Bởi vì mỗi kết quả trùng khớp hợp lệ tương ứng với chính xác một vị trí xóa theo ít nhất một cách và các bản sao được lọc trong một truy vấn nên mỗi kết quả trùng khớp chỉ được tính một lần và chỉ một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    st = set()

    for _ in range(n):
        st.add(input().strip())

    q = int(input())
    for _ in range(q):
        t = input().strip()
        seen = set()
        ans = 0

        for i in range(m + 1):
            cand = t[:i] + t[i+1:]
            if cand in st and cand not in seen:
                seen.add(cand)
                ans += 1

        print(ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên lưu trữ tất cả các chuỗi từ điển trong một tập hợp băm. Mỗi truy vấn sau đó xây dựng tất cả độ dài có thể-$m$chuỗi được hình thành bằng cách xóa một ký tự. các`seen`set đảm bảo rằng việc xóa lặp đi lặp lại tạo ra cùng một chuỗi không làm tăng câu trả lời. Điều này rất cần thiết khi truy vấn chứa các ký tự lặp lại vì các vị trí xóa khác nhau có thể mang lại kết quả giống nhau. 

Thao tác cắt lát`t[:i] + t[i+1:]`trực tiếp thực hiện logic xóa và vì$m \le 10$, chi phí của nó là không đáng kể. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1
a
c
4
aa
ca
mm
cf
```Đối với mỗi truy vấn: 

| Truy vấn | tôi đã xóa | ứng cử viên | trong bộ | đã thấy trước đây | đếm | 
| --- | --- | --- | --- | --- | --- | 
| aa | 0 | một | vâng | không | 1 | 
| aa | 1 | một | vâng | vâng | 1 | 
| ca | 0 | một | vâng | không | 1 | 
| ca | 1 | c | vâng | không | 2 | 
| mm | 0 | m | không | - | 0 | 
| mm | 1 | m | không | - | 0 | 
| cf | 0 | f | không | - | 0 | 
| cf | 1 | c | vâng | không | 1 | 

Đầu ra:```
1
2
0
1
```Điều này xác nhận rằng việc xóa trùng lặp không đếm gấp đôi cùng một chuỗi và việc khớp đó chỉ phụ thuộc vào tư cách thành viên trong từ điển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm + qm)$| tập hợp xây dựng có kích thước đầu vào tuyến tính, tối đa mỗi truy vấn sẽ kiểm tra$m+1 \le 11$xóa | 
| Không gian |$O(n)$| bộ băm lưu trữ tất cả các chuỗi đầu vào | 

Các ràng buộc cho phép lên đến$10^5$chuỗi và truy vấn, nhưng độ dài chuỗi cố định nhỏ giúp giải pháp tuyến tính này dễ dàng đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    old = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old
    return out.strip()

# provided sample
assert run("""2 1
a
c
4
aa
ca
mm
cf
""") == "1\n2\n0\n1"

# minimum case
assert run("""1 1
a
1
aa
""") == "1"

# all identical strings
assert run("""3 2
ab
ab
ab
1
aab
""") == "1"

# no matches
assert run("""2 2
ab
cd
1
xyz
""") == "0"

# repeated letters causing duplicate deletions
assert run("""1 2
ab
1
aab
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi đơn | 1 | trận đấu cơ bản | 
| trùng lặp trong từ điển | 1 | xử lý các mục giống hệt nhau | 
| không có kết quả phù hợp | 0 | trường hợp tiêu cực | 
| các chữ cái lặp lại trong truy vấn | 1 | xử lý xóa trùng lặp | 

## Vỏ cạnh 

Một trường hợp tinh tế phát sinh khi truy vấn chứa các ký tự lặp lại, chẳng hạn như`"aab"`. Xóa thứ nhất hoặc thứ hai`'a'`tạo ra cùng một ứng cử viên`"ab"`. Không có`seen`được đặt, điều này sẽ đếm không chính xác cùng một chuỗi từ điển nhiều lần. 

Đối với đầu vào:```
1 2
ab
1
aab
```Thuật toán xử lý việc xóa: 

Việc xóa đầu tiên mang lại`"ab"`, nằm trong tập hợp, vì vậy số đếm trở thành 1. Việc xóa lần thứ hai sẽ mang lại`"ab"`một lần nữa, nhưng nó đã được đánh dấu trong`seen`, nên nó bị bỏ qua. Xóa lần thứ ba mang lại`"aa"`, không có trong tập hợp. Câu trả lời cuối cùng là đúng 1. 

Hành vi này đảm bảo tính chính xác ngay cả khi nhiều vị trí xóa được thu gọn vào cùng một chuỗi.
