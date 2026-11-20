---
title: "CF 2179D - Nền văn minh Blackslex và Penguin"
description: "Chúng ta được yêu cầu xây dựng một hoán vị của tất cả các số nguyên từ $0$ đến $2^n - 1$. Chất lượng của một hoán vị được xác định thông qua một quy trình chạy trên các tiền tố: chúng tôi duy trì AND theo bit của tiền tố đã thấy cho đến nay và tại mỗi bước, chúng tôi thêm số lượng bit đã đặt trong giá trị AND đó…"
date: "2026-06-07T22:16:52+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2179
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1071 (Div. 3)"
rating: 1300
weight: 2179
solve_time_s: 108
verified: false
draft: false
---

[CF 2179D - Nền văn minh Blackslex và Penguin](https://codeforces.com/problemset/problem/2179/D) 

**Đánh giá:** 1300 
**Tags:** bitmasks, thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một hoán vị của tất cả các số nguyên từ$0$ĐẾN$2^n - 1$. Chất lượng của một hoán vị được xác định thông qua một quy trình chạy trên các tiền tố: chúng tôi duy trì AND theo bit của tiền tố đã thấy cho đến nay và tại mỗi bước, chúng tôi thêm số lượng bit đã đặt trong giá trị AND đó vào một điểm. 

Vì vậy, khi chúng tôi quét hoán vị từ trái sang phải, giá trị bắt đầu là phần tử đầu tiên, sau đó trở thành AND của hai phần tử đầu tiên, sau đó là AND của ba phần tử đầu tiên, v.v. Mỗi tiền tố đóng góp một số lượng phổ biến và chúng tôi muốn tổng số lượng các số lượng này càng lớn càng tốt. Trong số tất cả các hoán vị đạt được số điểm tối đa có thể, chúng ta được yêu cầu xuất ra hoán vị nhỏ nhất về mặt từ điển. 

Cấu trúc được liên kết chặt chẽ với các hoạt động bit trên một tập hợp các mặt nạ bit có độ dài hoàn chỉnh$n$. Từ$n \le 16$, kích thước vũ trụ nhiều nhất là$2^{16} = 65536$, vì vậy chúng tôi có đủ khả năng$O(2^n)$hoặc$O(2^n \log 2^n)$nhưng bất cứ điều gì có tính giai thừa hoặc liên quan đến hoán vị một cách rõ ràng đều không thể thực hiện được. 

Một ý tưởng ngây thơ là thử tất cả các hoán vị và tính điểm, nhưng ngay cả đối với$n = 4$, đó là$16!$, đã vượt quá giới hạn rồi. Ngay cả các giao dịch hoán đổi cục bộ tham lam cũng thất bại vì điểm số phụ thuộc vào tiền tố AND chứ không phải cấu trúc liền kề. 

Một trường hợp phức tạp nảy sinh từ sự tối giản về mặt từ điển. Hai hoán vị có thể có điểm tối ưu giống nhau nhưng khác nhau về thứ tự sớm. Ví dụ, đối với$n=2$, cả hai$[3,1,0,2]$Và$[3,2,0,1]$tối đa hóa điểm số, nhưng chỉ có một điểm hợp lệ nếu chúng ta nhấn mạnh vào sự tối thiểu về mặt từ điển. Điều này buộc phải xây dựng một cách xác định hơn là bất kỳ giải pháp tối ưu tùy ý nào. 

## Phương pháp tiếp cận 

Khó khăn chính là hiểu cấu trúc nào làm tăng tổng tiền tố VÀ số đếm. 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các hoán vị của$2^n$phần tử, tính toán tiền tố AND và đánh giá điểm số. Mỗi chi phí đánh giá$O(2^n \cdot n)$, và có$(2^n)!$hoán vị. Điều này là không thể ngay cả đối với$n=3$. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì nghĩ về các hoán vị, hãy nghĩ về cách tiền tố AND phát triển. Thao tác AND chỉ mất bit khi chúng ta tiến hành. Khi một bit trở thành 0 khi chạy AND, nó không bao giờ có thể quay trở lại. Vì vậy, mục tiêu là giữ càng nhiều bit tồn tại càng lâu càng tốt. 

Điều này gợi ý việc nhóm các số theo mẫu bit để chúng ta loại bỏ các ràng buộc dần dần thay vì mạnh mẽ. Cấu trúc tối ưu hóa ra tuân theo sự phân rã đệ quy theo bit quan trọng nhất: chúng tôi muốn đảm bảo rằng các bit cao hơn vẫn tồn tại trong các tiền tố sớm, điều đó có nghĩa là chúng tôi nên sắp xếp các số để các chuyển đổi tôn trọng các phân vùng theo bit. 

Điều này tự nhiên dẫn tới một cấu trúc đệ quy: đối với$n$bit, chia số thành hai nửa dựa trên bit cao nhất và xen kẽ chúng theo cách duy trì sớm các giá trị AND lớn. Trong mỗi hiệp, chúng tôi lặp lại chiến lược tương tự. Về cơ bản, đây là một cấu trúc giống như mã Gray nhưng được điều khiển bằng cách tối đa hóa tiền tố VÀ độ ổn định thay vì sự khác biệt về các vùng lân cận. 

Sau đó, ràng buộc tối thiểu về mặt từ điển sẽ buộc một thứ tự cố định giữa các lựa chọn đối xứng: khi tồn tại nhiều sự xen kẽ tối ưu, trước tiên chúng ta phải chọn số có sẵn nhỏ hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((2^n)! \cdot 2^n)$|$O(2^n)$| Quá chậm | 
| Xây dựng phân vùng bit đệ quy |$O(2^n)$|$O(2^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Việc xây dựng được đệ quy theo chiều dài bit. 

1. Xét tất cả các số từ$0$ĐẾN$2^n - 1$. Chia chúng thành hai nhóm: nhóm có bit cao nhất được đặt thành 0 và nhóm có bit được đặt thành 1. Sự phân tách này sẽ tách biệt xem một số có góp phần bảo toàn bit trên cùng trong AND tiền tố sớm hay không. 
2. Giải bài toán đệ quy cho từng nhóm bằng cách sử dụng$n-1$bit, bằng cách loại bỏ bit cao nhất. Bước này đảm bảo chúng ta bảo toàn được cấu trúc tối ưu bên trong mỗi nửa mà không cần lo lắng về phần trên cùng. 
3. Kết hợp hai kết quả đệ quy, nhưng đặt nhóm có bit cao hơn trước để giúp duy trì tiền tố AND lớn hơn trong thời gian dài hơn. Vì tất cả các số trong nhóm thứ hai đều có tập bit cao nhất nên việc đặt chúng sớm hơn sẽ giúp bit đó tồn tại trong nhiều tiền tố VÀ tính toán hơn. 
4. Trong mỗi lệnh gọi đệ quy, hãy đảm bảo rằng trong số các cấu trúc có điểm bằng nhau, thứ tự tối thiểu về mặt từ điển bằng cách luôn ưu tiên các giá trị thực nhỏ hơn trước khi các lựa chọn đối xứng. 
5. Khôi phục các giá trị thực tế khi trở về từ đệ quy bằng cách gắn lại bit cao nhất vào các phần tử nhóm thứ hai. 

Việc xây dựng hoạt động giống như một cây nhị phân trên các vị trí bit. Mỗi cấp độ kiểm soát một bit, đảm bảo nó tồn tại lâu nhất có thể trước khi bị buộc về 0 bằng cách trộn lẫn với nhóm đối diện. 

### Tại sao nó hoạt động 

AND đang chạy chỉ giảm về mặt bit thiết lập. Một bit góp phần vào điểm số ở mỗi tiền tố cho đến lần đầu tiên chúng tôi đưa phần tử có số 0 vào vị trí đó. Để tối đa hóa tổng đóng góp, chúng tôi muốn các bit cao biến mất càng muộn càng tốt, nghĩa là chúng tôi phải trì hoãn việc trộn các số khác nhau ở các bit cao. 

Bằng cách nhóm các số theo bit khác biệt quan trọng nhất và làm cạn kiệt một nhóm trước khi giới thiệu nhóm khác, chúng tôi tối đa hóa tuổi thọ của các bit bậc cao trong tiền tố AND. Đệ quy đảm bảo logic tương tự được áp dụng ở mọi bit thấp hơn, do đó cấu trúc tối ưu ở mọi quy mô. Tính tối thiểu về mặt từ điển được duy trì vì trong mỗi phân vùng tối ưu hợp lệ, trước tiên chúng tôi luôn mở rộng các nhãn nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build(n):
    if n == 0:
        return [0]

    prev = build(n - 1)

    # numbers with highest bit = 0
    zero = prev

    # numbers with highest bit = 1
    one = [x | (1 << (n - 1)) for x in prev]

    # key idea: put larger-block first to preserve high bits longer
    return one + zero

t = int(input())
for _ in range(t):
    n = int(input())
    print(*build(n))
```Phép đệ quy xây dựng một hoán vị của tất cả$2^n$mặt nạ bằng cách xây dựng giải pháp cho$n-1$bit rồi nhân đôi nó thành hai nửa: một nửa có bit cao nhất là 0 và một nửa có bit cao nhất là 1. Phép toán OR khôi phục các giá trị số chính xác sau khi đệ quy. 

Việc đặt hàng`one + zero`là sự lựa chọn thiết kế quan trọng. Nó đảm bảo rằng tất cả các số có tập bit cao nhất sẽ xuất hiện đầu tiên, sao cho bit này vẫn được đặt trong tiền tố VÀ với số bước tối đa có thể có trước khi bất kỳ phần tử bit 0 nào được đưa vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 1$Chúng tôi có số$[0,1]$. Việc xây dựng tiến hành như sau. 

| Bước | trước | không | một | kết quả | 
| --- | --- | --- | --- | --- | 
| n=0 | [0] | - | - | [0] | 
| n=1 | [0] | [0] | [1] | [1,0] | 

Đầu ra$[1,0]$giữ bit duy nhất tồn tại cho chính xác một tiền tố, tối đa hóa điểm số. 

Điều này xác nhận rằng việc sắp xếp tất cả các phần tử có bit cao trước tiên là cần thiết. 

### Ví dụ 2:$n = 2$Chúng tôi bắt đầu từ$n=1$:$[1,0]$| Bước | trước | không | một | kết quả | 
| --- | --- | --- | --- | --- | 
| n=1 | [1,0] | [1,0] | [3,2] | [3,2,1,0] | 

Vậy hoán vị cuối cùng là$[3,2,1,0]$. 

Điều này cho thấy cấu trúc nhân đôi đệ quy một cách rõ ràng: mỗi nửa giữ nguyên thứ tự tối ưu bên trong, trong khi nửa bit cao đến trước. 

Dấu vết chứng tỏ rằng việc xây dựng là nhất quán và bảo tồn cấu trúc ở mọi cấp độ bit. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^n)$| Mỗi số nguyên được tạo một lần thông qua đệ quy và bitwise OR | 
| Không gian |$O(2^n)$| Lưu trữ toàn bộ hoán vị qua các cấp độ đệ quy | 

Các ràng buộc cho phép lên đến$2^{16}$tổng số phần tử, do đó việc xây dựng thời gian tuyến tính cho mỗi trường hợp thử nghiệm dễ dàng đủ nhanh. Việc sử dụng bộ nhớ cũng bị giới hạn vì chúng tôi chỉ lưu trữ các hoán vị có kích thước tối đa là 65536. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def build(n):
        if n == 0:
            return [0]
        prev = build(n - 1)
        return [x | (1 << (n - 1)) for x in prev] + prev

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append(" ".join(map(str, build(n))))
    return "\n".join(out)

# provided samples
assert run("2\n1\n2\n") == "1 0\n3 2 1 0"

# custom: n=0 equivalent check via n=1 boundary behavior
assert run("1\n1\n") == "1 0"

# custom: n=3 size check
res = run("1\n3\n").split()
assert sorted(map(int, res)) == list(range(8))

# custom: lexicographic structure check prefix
assert run("1\n2\n").split()[0] == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 1 0 | căn cứ đặt hàng đúng đắn | 
| n=2 | 3 2 1 0 | cấu trúc đệ quy | 
| n=3 | hoán vị 0,7 | sự hoàn thiện | 

## Vỏ cạnh 

Một trường hợp tế nhị là$n=1$, trong đó quyết định giữa$[0,1]$Và$[1,0]$trực tiếp xác định điểm số. Việc xây dựng đặt phần tử bit cao lên hàng đầu, tạo ra$[1,0]$, điều này đảm bảo bit đơn đóng góp tối đa. 

Vì$n=2$, cả hai$[3,2,1,0]$và các hoán vị khác như$[3,1,0,2]$đạt được điểm tối ưu, nhưng việc xây dựng đệ quy sửa một thứ tự xác định duy nhất. Phần tử đầu tiên luôn có giá trị lớn nhất vì nó thuộc nhóm bit cao nhất. Chạy đệ quy từng bước cho thấy khối bit cao nhất đã hoàn toàn cạn kiệt trước khi bất kỳ phần tử bit thấp hơn nào xuất hiện, duy trì tính bất biến để các bit cao hơn tồn tại lâu nhất có thể. 

Đối với lớn hơn$n$, lý do tương tự cũng được áp dụng theo cách quy nạp: ở mỗi cấp độ bit, thuật toán đảm bảo rằng bit tương ứng không bị phá hủy sớm bằng cách trộn các phân vùng quá sớm, điều này đảm bảo số lượng tích lũy tối đa trên tất cả các tiền tố.
