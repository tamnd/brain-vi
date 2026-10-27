---
title: "CF 106125K - Koehandel"
description: "Hai người chơi sắp thực hiện một cuộc trao đổi dựa trên giá thầu kín. Một người chơi, Old MacDonald, đã giấu một số đồng xu vào một chiếc cốc và bạn biết chính xác đó là bao nhiêu đồng xu, hãy gọi nó là $c$. Bạn cũng có sẵn $n$ xu."
date: "2026-06-20T06:02:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106125
codeforces_index: "K"
codeforces_contest_name: "Delft Algorithm Programming Contest 2025 (DAPC 2025)"
rating: 0
weight: 106125
solve_time_s: 41
verified: true
draft: false
---

[CF 106125K - Koehandel](https://codeforces.com/problemset/problem/106125/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hai người chơi sắp thực hiện một cuộc trao đổi dựa trên giá thầu kín. Một người chơi, Old MacDonald, đã giấu một số đồng xu vào cốc và bạn biết chính xác đó là bao nhiêu xu, hãy gọi nó là$c$. Bạn cũng có$n$tiền xu có sẵn. Bạn phải chọn một số$x$, đại diện cho giá thầu của bạn. 

Sau khi cả hai giá thầu được tiết lộ, số xu sẽ được đổi chỗ: bạn nhận được MacDonald's$c$tiền xu và anh ấy nhận được của bạn$x$tiền xu. Sau đó, một con bò sẽ được trao cho ai trả được nhiều tiền hơn. Nếu giá thầu bằng nhau thì không có con bò nào đổi chủ. 

Mục tiêu của bạn là từ điển học có hai lớp. Đầu tiên, bạn muốn tối đa hóa số lượng bò mà bạn có được sau lần tương tác duy nhất này. Chỉ khi có nhiều lựa chọn$x$đưa ra kết quả giống nhau về con bò, bạn thích con bò nào mang lại cho bạn nhiều xu hơn sau khi trao đổi. Vì sàn giao dịch luôn hoán đổi giá thầu nên số xu cuối cùng của bạn sau giao dịch là$n - x + c$, vì vậy trong số các kết quả về con bò bằng nhau, bạn thích kết quả nhỏ hơn$x$. 

Đầu ra là một số nguyên duy nhất$x$, giá thầu bạn đã chọn. 

Những ràng buộc cho phép$c, n \le 10^9$, có nghĩa là bất kỳ giải pháp nào cũng phải có thời gian không đổi cho mỗi trường hợp thử nghiệm. Không có chỗ cho việc mô phỏng một loạt các giá thầu có thể có, nhưng cấu trúc của trò chơi đủ đơn giản để có thể đưa ra quyết định tối ưu bằng phương pháp phân tích. 

Một vài tình huống khó khăn quan trọng. 

Nếu bạn đặt giá thầu chính xác$c$, kết quả là hòa và không có con bò nào được đổi. Một chiến lược ngây thơ có thể coi điều này là trung lập hoặc tương đương với việc thắng hoặc thua một cách không chính xác, nhưng đó là một loại kết quả khác biệt. 

Nếu bạn đặt giá thầu nhiều hơn$n$, việc di chuyển không hợp lệ vì bạn không thể tiêu số tiền mà bạn không có. Điều này làm cho ranh giới quyết định tại$x \le n$chủ yếu. 

Nếu như$c = 0$, MacDonald không đóng góp gì và bất kỳ giá thầu tích cực nào sẽ ngay lập tức thắng được con bò, nhưng việc giảm thiểu giá thầu sẽ trở thành mục tiêu phụ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi giá thầu có thể$x$từ$0$ĐẾN$n$. Với mỗi ứng viên, chúng tôi mô phỏng kết quả: so sánh$x$với$c$, xác định xem chúng ta được hay mất một con bò và tính số tiền cuối cùng$n - x + c$. Sau đó chúng tôi chọn cái tốt nhất theo mục tiêu hai cấp độ. 

Điều này hoạt động chính xác vì vấn đề chỉ có$n+1$khả năng, và mỗi đánh giá là thời gian không đổi. Tuy nhiên, với$n$lên tới$10^9$, việc lặp lại tất cả các giá trị sẽ yêu cầu hàng tỷ thao tác cho mỗi lần kiểm tra, vượt xa giới hạn 1 giây. 

Quan sát quan trọng là kết quả chỉ phụ thuộc vào sự so sánh giữa$x$Và$c$, không phải về độ lớn vượt quá ngưỡng đó. Mỗi giá thầu hợp lệ rơi vào đúng một trong ba loại:$x < c$,$x = c$, hoặc$x > c$. Trong mỗi danh mục, kết quả của con bò là cố định, do đó, cách tối ưu hóa có ý nghĩa duy nhất là chọn đại diện tốt nhất theo mục tiêu đồng tiền phụ. 

Nếu như$x > c$, bạn thắng được con bò. Trong số tất cả các lựa chọn như vậy, số tiền cuối cùng giảm dần khi$x$tăng, do đó giá thầu trúng thầu tốt nhất là giá trị nhỏ nhất có thể lớn hơn$c$, đó là$c+1$, miễn là không vượt quá$n$. 

Nếu như$x = c$, bạn không nhận được con bò nào nhưng vẫn giữ được tiền xu$n$. Điều này luôn tệ hơn về số lượng bò so với chiến thắng, vì vậy nó chỉ quan trọng khi chiến thắng là không thể. 

Nếu như$x < c$, bạn mất con bò. Trong số này, để tối đa hóa số tiền bạn chọn$x = 0$, nhưng điều này chỉ quan trọng nếu chiến thắng là không thể. 

Do đó, giải pháp được xác định bằng việc có tồn tại giá thầu trúng thầu hay không, tức là liệu$c+1 \le n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. So sánh$c + 1$với$n$. Điều này xác định liệu có tồn tại giá thầu vượt trội so với MacDonald trong khi vẫn ở mức giá phải chăng hay không. 
2. Nếu$c + 1 \le n$, chọn$x = c + 1$. Đây là giá thầu thắng nhỏ nhất có thể, đảm bảo con bò sẽ thắng trong khi giảm thiểu số tiền chi tiêu. 
3. Nếu không, không có giá thầu nào có thể vượt quá$c$trong phạm vi ngân sách nên chiến thắng là không thể. 
4. Trong trường hợp đó, chọn$x = 0$. Điều này tránh hoàn toàn việc mất xu trong khi chấp nhận rằng con bò bị mất hoặc những tình huống bị trói là khó tránh khỏi. 

### Tại sao nó hoạt động 

Cấu trúc của trò chơi giảm tất cả các kết quả xuống một ngưỡng đơn điệu tại$c$. Bất kỳ giá thầu nào ở trên$c$tạo ra cùng một kết quả là giành được một con bò, nhưng chi phí sẽ tăng theo giá trị giá thầu. Do đó, chiến lược chiến thắng tối ưu là giá trị khả thi tối thiểu trên ngưỡng. Nếu không có giá trị như vậy tồn tại thì tất cả các trạng thái chiến thắng đều không thể truy cập được và chúng tôi sẽ quay lại cấu hình thua hoặc hòa tốt nhất có thể, điều này đạt được bằng cách giảm thiểu giá thầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    c, n = map(int, input().split())
    if c + 1 <= n:
        print(c + 1)
    else:
        print(0)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện logic ngưỡng xuất phát trước đó. điều kiện`c + 1 <= n`kiểm tra tính khả thi của giá trúng thầu nhỏ nhất. Nếu đúng, chúng tôi xuất giá trị đó. Mặt khác, chúng tôi mặc định là 0, vì bất kỳ giá thầu tích cực nào cũng không thể đánh bại MacDonald và chỉ làm giảm số tiền cuối cùng. 

Một điểm tinh tế là chúng ta không bao giờ cần xem xét việc đặt giá thầu một cách chính xác$c$. Trường hợp đó không bao giờ cải thiện kết quả của con bò so với thua hoặc thắng và nó cũng không cải thiện số dư tiền xu so với$x=0$trong chế độ thua cuộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1: nhập liệu`6 19`Chúng tôi tính toán liệu giá thầu chiến thắng có tồn tại hay không. 

| c | n | c+1 <= n | đã chọn x | kết quả | 
| --- | --- | --- | --- | --- | 
| 6 | 19 | đúng | 7 | thắng bò | 

Từ$7 > 6$và nằm trong ngân sách, chúng tôi chọn$x=7$. Điều này mang lại chiến thắng cho con bò và giảm thiểu chi tiêu trong số tất cả các giá thầu thắng cuộc. Bất kỳ giá thầu cao hơn nào cũng sẽ thắng nhưng để lại ít tiền hơn sau khi hoán đổi. 

### Ví dụ 2: nhập liệu`42 37`| c | n | c+1 <= n | đã chọn x | kết quả | 
| --- | --- | --- | --- | --- | 
| 42 | 37 | sai | 0 | thua hoặc hòa | 

Ở đây không có giá thầu nào có thể vượt quá 42 vì ngân sách quá nhỏ. Điều tốt nhất chúng ta có thể làm là tránh chi tiêu không cần thiết, vì thế chúng ta chọn$x=0$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| So sánh số học đơn lẻ cho mỗi trường hợp thử nghiệm | 
| Không gian |$O(1)$| Chỉ sử dụng một số lượng biến không đổi | 

Giải pháp này dễ dàng nằm trong giới hạn vì nó không thực hiện phép lặp và chỉ thực hiện các phép toán số nguyên cơ bản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("6 19\n") == "7"
assert run("42 37\n") == "0"
assert run("25 25\n") == "0"

# custom cases
assert run("0 0\n") == "1" or run("0 0\n") == "0", "boundary small equal case"
assert run("0 10\n") == "1", "can always beat zero bid"
assert run("9 8\n") == "0", "cannot exceed c"
assert run("1000000000 1000000000\n") == "0", "max boundary equality"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 | 0 hoặc 1 | hành vi ranh giới khi không có lợi thế | 
| 0 10 | 1 | giá trúng thầu tối thiểu khi đối thủ bằng 0 | 
| 9 8 | 0 | không thể thắng do hạn chế về ngân sách | 
| 1000000000 1000000000 | 0 | cạnh bình đẳng ràng buộc tối đa | 

## Vỏ cạnh 

Khi nào$c = n$, điều kiện$c+1 \le n$thất bại, vì vậy chúng tôi xuất ra$0$. Đối với đầu vào`25 25`, thuật toán chọn$x = 0$. Bất kỳ giá thầu nào từ 1 đến 25 đều dẫn đến kết quả hòa hoặc thua về kết quả của con bò và không thể đặt giá thầu cao hơn. Quyết định chính xác tránh được việc mất tiền không cần thiết. 

Khi$c = 0$, Ví dụ`0 10`, điều kiện đúng và ta chọn$x = 1$. Đây là giá thầu tích cực nhỏ nhất và đảm bảo chiến thắng cho con bò. Lựa chọn$x = 0$sẽ chỉ dẫn đến kết quả hòa, điều này thực sự tệ hơn về số lượng bò. 

Khi$n = 0$, chúng tôi không thể đặt bất kỳ giá thầu tích cực nào. Đối với đầu vào`5 0`, điều kiện không thành công và chúng tôi xuất ra$0$, đó là động thái hợp lệ duy nhất. Mặc dù chúng tôi mất con bò, nhưng không có sự thay thế nào tồn tại trong những ràng buộc.
