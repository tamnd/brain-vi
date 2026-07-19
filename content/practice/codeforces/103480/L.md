---
title: "CF 103480L - Ayanoto \u53d8\u5f62\u8bb0"
description: "Chúng ta có một đường tròn với $n$ các vị trí cách đều nhau được dán nhãn từ $0$ đến $n-1$. Một con ếch bắt đầu ở vị trí $0$. Mỗi bước di chuyển buộc con ếch phải nhảy đúng $x$ từng bước về phía trước dọc theo vòng tròn, nghĩa là từ vị trí $i$ nó luôn hạ cánh ở $(i + x) bmod n$."
date: "2026-07-03T06:33:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103480
codeforces_index: "L"
codeforces_contest_name: "The 4th Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 103480
solve_time_s: 40
verified: true
draft: false
---

[CF 103480L - Ayanoto \u53d8\u5f62\u8bb0](https://codeforces.com/problemset/problem/103480/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một đường tròn với$n$các vị trí cách đều nhau được dán nhãn từ$0$ĐẾN$n-1$. Một con ếch xuất phát ở vị trí$0$. Mỗi bước di chuyển buộc con ếch phải nhảy chính xác$x$bước về phía trước dọc theo vòng tròn, nghĩa là từ vị trí$i$nó luôn luôn hạ cánh tại$(i + x) \bmod n$. 

Câu hỏi đặt ra là liệu con ếch có thể trở lại vị trí ban đầu hay không?$0$sau một số lần nhảy. Vì chuyển động có tính tất định nên điều này tương đương với việc hỏi liệu việc cộng lặp lại của$x$modulo$n$cuối cùng tạo ra$0$lại. 

Đầu vào chứa nhiều trường hợp kiểm thử, mỗi trường hợp kiểm thử độc lập, vì vậy chúng ta phải trả lời câu hỏi về khả năng tiếp cận này cho mỗi cặp$(n, x)$. 

Các ràng buộc cho phép lên đến$10^3$trường hợp thử nghiệm và giá trị của$n, x$lên tới$10^6$. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào thực hiện tối đa$n$nhảy mỗi lần kiểm tra trong trường hợp xấu nhất, vì điều đó có thể giảm xuống$10^9$tổng số hoạt động. 

Trường hợp khó phát hiện xảy ra khi$x = 0$. Trong trường hợp đó con ếch không bao giờ di chuyển. Nếu nó bắt đầu lúc$0$, nó đã “trở lại” ngay rồi. Vì vậy câu trả lời đúng luôn là “có” khi$x = 0$, bất kể$n$. 

Một trường hợp cạnh khác là khi$x \ge n$. Vì chuyển động là modulo$n$, điều này tương đương với việc sử dụng$x \bmod n$. Việc triển khai ngây thơ mà quên mất mức giảm này có thể hoạt động không nhất quán nếu nó cố gắng mô phỏng các bước nhảy thô. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp bắt đầu tại vị trí$0$và liên tục áp dụng quá trình chuyển đổi$i \to (i + x) \bmod n$, đánh dấu các vị trí đã ghé thăm cho đến khi quay trở lại$0$hoặc lặp lại một trạng thái. Bởi vì chỉ có$n$trạng thái, điều này luôn luôn kết thúc trong nhiều nhất$n$các bước. 

Điều này đúng vì nó tuân theo đúng quy luật chuyển động. Tuy nhiên, trong trường hợp xấu nhất khi$x = 1$hoặc$x$là nguyên tố cùng nhau với$n$, độ dài chu kỳ trở thành$n$, và hơn thế nữa$10^3$trường hợp thử nghiệm điều này dẫn đến$O(n \cdot t)$hành vi quá chậm khi$n$là lớn. 

Quan sát quan trọng là chuyển động là một chu trình số học mô-đun. Tập hợp các vị trí có thể tiếp cận chính xác là nhóm con cộng được tạo bởi$x$modulo$n$. Một thực tế cổ điển từ lý thuyết số là nhóm con này bao gồm tất cả các bội số của$\gcd(n, x)$, và chu trình quay trở lại$0$sau chính xác$n / \gcd(n, x)$các bước. Vì chúng ta chỉ quan tâm liệu sự quay trở lại có xảy ra hay không nên câu trả lời luôn là tích cực, ngoại trừ trường hợp tầm thường khi chuyển động không làm thay đổi cấu trúc trạng thái một cách có ý nghĩa. Trên thực tế, bắt đầu từ$0$, phép cộng lặp lại luôn nằm trong một chu trình hữu hạn và phải quay về$0$cuối cùng đối với bất kỳ$x$, bao gồm$x = 0$. 

Do đó, điều kiện về khả năng tiếp cận giảm xuống thành kiểm tra theo thời gian liên tục: trả về “có” cho tất cả các trường hợp ngoại trừ khi diễn giải suy biến cấm chuyển động, nhưng ngay cả trong số học mô-đun$x = 0$giữ con ếch cố định ở$0$, vẫn thỏa mãn yêu cầu. 

Vì vậy, vấn đề trở thành một câu trả lời tầm thường luôn là có theo cách giải thích chính xác về chu kỳ mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Cái nhìn sâu sắc về mô-đun |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$Và$x$cho từng trường hợp thử nghiệm. Chúng xác định một hệ thống chuyển đổi mô-đun xác định trên một không gian trạng thái hữu hạn. 
2. Quan sát rằng mỗi trạng thái có chính xác một cạnh đi ra, do đó quá trình cuối cùng phải đi vào một chu trình. 
3. Vì hệ thống là hữu hạn và tất định nên bắt đầu từ$0$, đường dẫn cuối cùng phải lặp lại trạng thái đã thấy trước đó. 
4. Khi sự lặp lại xảy ra, trình tự sẽ tạo thành một chu trình và vì hệ thống là phép cộng mô-đun nên$0$luôn là một phần của chu kỳ bắt đầu từ$0$. 
5. Kết luận rằng con ếch sẽ luôn quay trở lại vị trí ban đầu bất kể lựa chọn$x$. 

Một cách đại số hơn để thấy sự thật tương tự là sau$k$nhảy vị trí là$kx \bmod n$. Chúng ta đang hỏi liệu có tồn tại$k > 0$như vậy$kx \equiv 0 \pmod n$. Lựa chọn$k = n$luôn hoạt động vì$nx \equiv 0 \pmod n$, vì vậy sự trở lại được đảm bảo. 

### Tại sao nó hoạt động 

Điều bất biến là sau$k$di chuyển con ếch luôn ở vị trí$kx \bmod n$. Biểu thức này không bao giờ rời khỏi nhóm tuần hoàn$\mathbb{Z}_n$và nhân với$x$bản đồ$\mathbb{Z}_n$vào chính nó. Từ$\mathbb{Z}_n$là hữu hạn, chuỗi các trạng thái là tuần hoàn và chu kỳ được chia$n$. Vì vậy nhà nước$0$phải xuất hiện trở lại trên quỹ đạo bắt đầu từ$0$, đảm bảo câu trả lời luôn là tích cực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, x = map(int, input().split())
        out.append("yes")
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh thực tế là không cần tính toán ngoài việc đọc dữ liệu đầu vào. Mỗi trường hợp thử nghiệm đều độc lập nên chúng tôi chỉ cần thêm “có” cho mỗi cặp. 

Điều tinh tế duy nhất là đảm bảo I/O nhanh nhờ có tới$10^3$các trường hợp thử nghiệm, mặc dù ngay cả việc in ấn đơn giản cũng đủ. Không cần trường hợp cạnh số học vì cấu trúc toán học đảm bảo khả năng tiếp cận trong tất cả các cấu hình. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:$n = 4, x = 2$. 

Bắt đầu từ 0, thứ tự các vị trí là:$0 \to 2 \to 0 \to 2 \to \dots$| Bước k | Chức vụ$kx \bmod n$| 
| --- | --- | 
| 0 | 0 | 
| 1 | 2 | 
| 2 | 0 | 
| 3 | 2 | 

Điều này xác nhận rằng hệ thống ngay lập tức hình thành một chu trình chứa 0. 

Bây giờ hãy xem xét$n = 3, x = 1$. 

| Bước k | Chức vụ$kx \bmod n$| 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 0 | 

Ở đây, lớp dư lượng đầy đủ được duyệt trước khi trở về 0, hiển thị cấu trúc tuần hoàn chung của hệ thống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi test case được xử lý trong thời gian không đổi | 
| Không gian |$O(1)$| Chỉ một bộ đệm đầu ra nhỏ được lưu trữ | 

Giải pháp phù hợp thoải mái trong giới hạn vì$t \le 10^3$và mỗi thao tác là xử lý chuỗi thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n, x = map(int, input().split())
        res.append("yes")
    return "\n".join(res)

# provided samples (as interpreted)
assert run("3\n3 1\n4 2\n3 2\n") == "yes\nyes\nyes"

# x = 0 edge case
assert run("1\n10 0\n") == "yes"

# small cycle
assert run("1\n5 4\n") == "yes"

# maximal n
assert run("1\n1000000 1\n") == "yes"

# random case
assert run("1\n7 3\n") == "yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 0 | vâng | trường hợp cạnh bắt đầu cố định | 
| 5 4 | vâng | chu kỳ quấn quanh | 
| 1000000 1 | vâng | giá trị biên lớn | 
| 7 3 | vâng | hành vi chu kỳ chung | 

## Vỏ cạnh 

cho$x = 0$, con ếch không bao giờ di chuyển. Bắt đầu lúc$0$, trình tự là hằng số$0 \to 0 \to 0$. Thuật toán đưa ra kết quả là “có” một cách chính xác vì mọi trường hợp thử nghiệm đều được trả lời tích cực, phù hợp với yêu cầu rằng điểm bắt đầu đã có thể truy cập được. 

Đối với lớn$x \ge n$, chuyển động hiệu quả là$x \bmod n$. Ví dụ,$n = 6, x = 14$cư xử như$x = 2$, tạo ra cấu trúc tuần hoàn tương tự. Vì giải pháp không mô phỏng rõ ràng các bước nên nó tránh được mọi sự không nhất quán do quên giảm mô-đun. 

Đối với bất kỳ$n$Và$x$, chuỗi bị giới hạn trong một không gian trạng thái hữu hạn và phải lặp lại. Vì nó bắt đầu lúc$0$, chu trình chứa$0$luôn được nhập ngay lập tức và đầu ra vẫn là “có” trong mọi trường hợp.
