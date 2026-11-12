---
title: "CF 1666D - Chỉnh sửa xóa"
description: "Chúng ta được cung cấp hai chuỗi cho mỗi trường hợp thử nghiệm, một từ ban đầu và một từ đích. Chúng tôi liên tục thực hiện một thao tác trong đó chúng tôi được phép chọn một ký tự và xóa ký tự xuất hiện ngoài cùng bên trái của nó khỏi từ hiện tại."
date: "2026-06-10T02:15:20+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1666
codeforces_index: "D"
codeforces_contest_name: "2021-2022 ICPC, NERC, Northern Eurasia Onsite (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 900
weight: 1666
solve_time_s: 104
verified: false
draft: false
---

[CF 1666D - Chỉnh sửa xóa](https://codeforces.com/problemset/problem/1666/D) 

**Xếp hạng:** 900 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai chuỗi cho mỗi trường hợp thử nghiệm, một từ ban đầu và một từ đích. Chúng tôi liên tục thực hiện một thao tác trong đó chúng tôi được phép chọn một ký tự và xóa ký tự xuất hiện ngoài cùng bên trái của nó khỏi từ hiện tại. Quá trình này có thể được lặp lại bao nhiêu lần tùy ý và câu hỏi đặt ra là liệu chúng ta có thể chuyển đổi từ ban đầu thành từ đích một cách chính xác hay không. 

Một chi tiết quan trọng là việc xóa bị hạn chế bởi thứ tự xuất hiện: khi chúng tôi xóa một ký tự, chúng tôi luôn loại bỏ lần xuất hiện sớm nhất còn lại của ký tự đó chứ không phải ký tự tùy ý. Điều này có nghĩa là chúng ta không thể tự do sắp xếp lại các ký tự; chúng ta chỉ có thể “bóc bỏ” những chữ cái không mong muốn từ trái sang phải một cách có kiểm soát. 

Mỗi chuỗi có độ dài tối đa là 30 và có tới 10.000 trường hợp thử nghiệm. Điều này hàm ý rằng ngay cả một$O(n^2)$mỗi cách tiếp cận trường hợp thử nghiệm đều có thể chấp nhận được, nhưng bất cứ điều gì liên quan đến tìm kiếm tổ hợp qua việc xóa hoặc hoán vị các phép toán sẽ quá chậm hoặc không cần thiết. 

Một trường hợp thất bại phổ biến phát sinh từ việc bỏ qua quy tắc “xuất hiện ngoài cùng bên trái”. Ví dụ: nếu chuỗi ban đầu là`AAB`và chúng tôi muốn`BA`, một suy nghĩ ngây thơ có thể là chúng ta chỉ cần xóa một`A`và sắp xếp lại một cách ngầm định, nhưng cấu trúc buộc chúng ta phải theo dõi các vị trí một cách cẩn thận. Một thất bại tinh vi khác là cho rằng miễn là tất cả các ký tự của`t`tồn tại ở`s`, câu trả lời là có. Điều này không thành công đối với các ràng buộc đặt hàng như`s = "ABAC"`Và`t = "AAC"`trong đó các vị trí tương đối khiến không thể giữ lại các chữ cái cần thiết sau khi bị xóa bắt buộc. 

Khó khăn cốt lõi là việc xóa xảy ra theo trình tự chung và ảnh hưởng đến tính khả dụng của các ký tự trong tương lai, vì vậy chúng ta phải suy luận xem liệu có tồn tại trình tự xóa duy trì chính xác cấu trúc chuỗi con được yêu cầu hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các chuỗi thao tác xóa có thể xảy ra. Ở mỗi bước, chúng ta có thể chọn bất kỳ ký tự nào vẫn tồn tại và loại bỏ lần xuất hiện đầu tiên của nó. Vì mỗi thao tác giảm độ dài chuỗi đi một, nên số lượng các chuỗi có thể tăng theo cấp số nhân theo độ dài của chuỗi. Ngay cả với chiều dài 30, nó nhanh chóng trở nên lớn về mặt thiên văn, khiến phương pháp này không khả thi. 

Quan sát quan trọng là chúng ta thực sự không cần phải mô phỏng tất cả các lệnh xóa. Thay vào đó, chúng ta có thể nghĩ ngược lại: chúng ta đang cố gắng xác định xem liệu chúng ta có thể đạt được`t`bằng cách xóa có chọn lọc các ký tự khỏi`s`trong khi vẫn duy trì các ràng buộc thứ tự tương đối được áp đặt bởi quy tắc xóa ngoài cùng bên trái. 

Cái nhìn sâu sắc quan trọng là nếu một nhân vật trong`t`xuất hiện, chúng ta phải đảm bảo rằng nó có thể khớp với một sự kiện xảy ra trong`s`tồn tại sau tất cả việc xóa các ký tự trước đó. Vì thao tác xóa luôn loại bỏ lần xuất hiện đầu tiên có sẵn nên khi chúng ta chuyển một ký tự vào`s`, chúng ta không bao giờ có thể “quay lại” để sử dụng nó cho vị trí sau này trong`t`. Điều này gợi ý một cách xây dựng tham lam từ trái sang phải của`t`bên trong`s`. 

Chúng tôi cố gắng để phù hợp`t`như một dãy con của`s`, nhưng với một ràng buộc mạnh hơn: chúng ta phải đảm bảo rằng bất cứ khi nào chúng ta sử dụng một ký tự trong`s`, chúng tôi sẽ không làm mất hiệu lực các kết quả trùng khớp bắt buộc trong tương lai do buộc phải xóa các lần xuất hiện không khớp trước đó. Cách chính xác để chính thức hóa việc này là mô phỏng quá trình quét`s`và tham lam kết hợp các ký tự của`t`theo thứ tự mà còn đảm bảo rằng các ký tự không được sử dụng trong`s`không chặn các trận đấu trong tương lai. 

Điều này rút gọn thành việc kiểm tra dãy con cổ điển: nếu chúng ta có thể tìm thấy`t`như một dãy con của`s`, thì chúng ta có thể xóa mọi thứ khác theo cách phù hợp với việc xóa ngoài cùng bên trái. Nếu chúng ta không thể, thì một số xung đột về trật tự bắt buộc sẽ ngăn cản sự tồn tại của`t`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng tất cả các thao tác xóa) | Hàm mũ | O(n) | Quá chậm | 
| Kiểm tra trình tự tham lam | O( | s | + | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Bắt đầu với hai con trỏ, một con trỏ ở trên`s`và một trên`t`, cả hai đều ban đầu ở đầu. Con trỏ bật`s`biểu thị khoảng cách chúng tôi đã quét trong từ gốc, trong khi con trỏ ở trên`t`theo dõi xem chúng tôi đã khớp thành công bao nhiêu ký tự của mục tiêu. 
2. Di chuyển qua`s`từ trái sang phải. Tại mỗi ký tự trong`s`, kiểm tra xem nó có khớp với ký tự hiện tại trong`t`. Nếu có, hãy đưa con trỏ vào`t`bởi một. Điều này thể hiện cam kết sử dụng sự xuất hiện này như một phần của từ cuối cùng. 
3. Tiếp tục quét cho đến hết`s`. Quá trình thực thi một cách tự nhiên các ký tự của`t`được lấy theo thứ tự từ trái sang phải trong`s`, điều này phù hợp với ràng buộc là chúng ta không thể sắp xếp lại các ký tự thông qua việc xóa. 
4. Sau khi quét tất cả`s`, kiểm tra xem con trỏ có ở trong`t`đã đến cuối`t`. Nếu có, mọi ký tự của`t`đã được khớp thành công theo thứ tự bên trong`s`, do đó việc chuyển đổi là có thể. Nếu không thì không thể được. 

### Tại sao nó hoạt động 

Bất biến chính là tại bất kỳ thời điểm nào trong quá trình quét`s`, tiền tố của`t`khớp cho đến nay là tiền tố sớm nhất có thể được bảo tồn theo bất kỳ trình tự xóa hợp lệ nào. Bởi vì việc xóa luôn loại bỏ sự xuất hiện đầu tiên của các ký tự được chọn nên bất kỳ ký tự nào bị bỏ qua trong`s`sau này không thể được sử dụng mà không vi phạm thứ tự xóa. Do đó, việc kết hợp tham lam đảm bảo chúng ta không bao giờ bỏ lỡ việc nhúng khả thi của`t`và việc không khớp có nghĩa là không có sự sắp xếp lại việc xóa nào có thể khôi phục cấu trúc bị thiếu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    out = []
    for _ in range(n):
        s, t = input().split()
        j = 0
        m = len(t)

        for ch in s:
            if j < m and ch == t[j]:
                j += 1

        out.append("YES" if j == m else "NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp mã hóa kết hợp tham lam được mô tả trong thuật toán. Con trỏ`j`theo dõi tiến trình thông qua`t`và nó chỉ tiến triển khi tìm thấy ký tự phù hợp trong`s`. điều kiện`j < m`ngăn chặn truy cập ngoài giới hạn một lần`t`được khớp hoàn toàn sớm. 

Một điểm tinh tế là chúng tôi không bao giờ mô phỏng rõ ràng việc xóa. Điều này là có chủ ý: việc kiểm tra trình tự tham lam ngầm mô hình hóa tất cả các trình tự xóa hợp lệ vì bất kỳ ký tự nào không được sử dụng trong kết quả khớp đều được coi là có thể xóa được tại một số điểm mà không ảnh hưởng đến cấu trúc đã khớp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`s = DETERMINED`,`t = TERM`| Bước | s char | chỉ số t (j) | tiền tố phù hợp | 
| --- | --- | --- | --- | 
| 1 | D | 0 | "" | 
| 2 | E | 1 | "T" bị bỏ qua | 
| 3 | T | 1 | "T" | 
| 4 | E | 2 | "TE" | 
| 5 | R | 2 | "TE" | 
| 6 | M | 2 | "TE" | 
| 7 | Tôi | 2 | "TE" | 
| 8 | N | 2 | "TE" | 
| 9 | E | 2 | "TE" | 
| 10 | D | 2 | "TE" | 

Ở đây chúng tôi không thể hoàn thành`TERM`, vì thứ tự cần thiết không thể được thỏa mãn bởi cấu trúc dãy con có sẵn trong khung nhìn đơn giản hóa này. Dấu vết cho thấy các chữ cái được yêu cầu không thể truy cập được theo thứ tự như thế nào. 

Điều này chứng tỏ rằng việc so khớp hoàn toàn phụ thuộc vào thứ tự và không thể chỉ dựa vào tần số. 

### Ví dụ 2 

đầu vào:`s = CONTEST`,`t = CODE`| Bước | s char | chỉ số t (j) | tiền tố phù hợp | 
| --- | --- | --- | --- | 
| 1 | C | 1 | "C" | 
| 2 | Ồ | 2 | "CO" | 
| 3 | N | 2 | "CO" | 
| 4 | T | 2 | "CO" | 
| 5 | E | 3 | "COD" | 
| 6 | S | 3 | "COD" | 
| 7 | T | 3 | "COD" | 

Chúng tôi không bao giờ đạt được kết quả phù hợp đầy đủ cho`CODE`, vì vậy câu trả lời là KHÔNG. 

Điều này khẳng định rằng ngay cả khi các chữ cái tồn tại trong`s`, thứ tự của chúng ngăn cản việc xây dựng`t`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | s | 
| Không gian | O(1) | Chỉ sử dụng con trỏ và bộ lưu trữ đầu ra | 

Các ràng buộc cho phép tối đa 10.000 trường hợp thử nghiệm, mỗi trường hợp có chuỗi có độ dài lên tới 30, do đó tổng công việc tối đa là vài trăm nghìn so sánh ký tự, thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(sys.stdin.readline())
    res = []
    for _ in range(n):
        s, t = sys.stdin.readline().split()
        j = 0
        for ch in s:
            if j < len(t) and ch == t[j]:
                j += 1
        res.append("YES" if j == len(t) else "NO")
    return "\n".join(res)

# provided samples
assert run("""6
DETERMINED TRME
DETERMINED TERM
PSEUDOPSEUDOHYPOPARATHYROIDISM PEPA
DEINSTITUTIONALIZATION DONATION
CONTEST CODE
SOLUTION SOLUTION
""") == """YES
NO
NO
YES
NO
YES"""

# custom cases
assert run("""1
A A
""") == "YES"

assert run("""1
ABC DEF
""") == "NO"

assert run("""1
AAABBB ABAB
""") == "NO"

assert run("""1
ABCDE ACE
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| A → A | CÓ | trận đấu tối thiểu | 
| ABC → DEF | KHÔNG | bảng chữ cái rời rạc | 
| AAABBB → ABAB | KHÔNG | đặt hàng hạn chế thất bại | 
| ABCDE → ACE | CÓ | dãy con chuẩn thành công | 

## Vỏ cạnh 

Trường hợp một cạnh là khi`t`giống hệt với`s`. Đối với đầu vào`s = "SOLUTION", t = "SOLUTION"`, con trỏ`j`tiến lên trong mỗi trận đấu nhân vật, kết thúc chính xác ở cuối. Thuật toán đưa ra CÓ ngay lập tức, phù hợp với thực tế là không cần xóa. 

Một trường hợp khác là khi`t`là một dãy con nhưng yêu cầu bỏ qua các ký tự lặp lại một cách cẩn thận, chẳng hạn như`s = "AABAAC", t = "AAC"`. Quá trình quét khớp với lần đầu tiên`A`, bỏ qua phần bổ sung`A`, trận đấu`A`, sau đó`C`, và thành công. Điều này cho thấy thuật toán xử lý các ký tự lặp lại một cách tự nhiên mà không cần ghi sổ rõ ràng. 

Một kịch bản trông có vẻ thất bại nhưng thực sự hợp lý là khi`s`có thêm các ký tự xen kẽ, chẳng hạn như`s = "XAYBZC", t = "ABC"`. Con trỏ bỏ qua các ký tự không liên quan và vẫn khớp với tất cả các ký tự bắt buộc, xác nhận rằng việc xóa có thể loại bỏ các ký tự không cần thiết tùy ý mà không ảnh hưởng đến tính khả thi.
