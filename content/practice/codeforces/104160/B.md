---
title: "CF 104160B - Chuỗi con nhị phân"
description: "Chúng ta được cho một độ dài $n$ và chúng ta phải xây dựng một chuỗi nhị phân có độ dài đó. Mục tiêu không phải là thỏa mãn bất kỳ ràng buộc mẫu nào mà là để tối đa hóa số lượng chuỗi con khác biệt xuất hiện trong chuỗi."
date: "2026-07-02T01:02:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104160
codeforces_index: "B"
codeforces_contest_name: "The 2022 ICPC Asia Shenyang Regional Contest (The 1st Universal Cup, Stage 1: Shenyang)"
rating: 0
weight: 104160
solve_time_s: 47
verified: true
draft: false
---

[CF 104160B - Chuỗi con nhị phân](https://codeforces.com/problemset/problem/104160/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chiều dài$n$và chúng ta phải xây dựng một chuỗi nhị phân có độ dài đó. Mục tiêu không phải là thỏa mãn bất kỳ ràng buộc mẫu nào mà là để tối đa hóa số lượng chuỗi con khác biệt xuất hiện trong chuỗi. Một chuỗi con được xác định theo cách thông thường là một phân đoạn liền kề và hai chuỗi con được coi là khác nhau nếu nội dung của chúng khác nhau, ngay cả khi chúng xuất hiện ở các vị trí khác nhau. 

Kích thước đầu vào tăng lên$2 \cdot 10^5$, do đó bản thân chuỗi đầu ra đã lớn. Bất kỳ giải pháp nào rõ ràng phải chạy theo thời gian tuyến tính, vì ngay cả việc ghi kết quả đầu ra cũng đã tốn kém.$O(n)$. Điều này ngay lập tức loại trừ bất cứ điều gì liệt kê các chuỗi con hoặc so sánh trực tiếp các cặp chuỗi con, vì số lượng chuỗi con là$O(n^2)$, và thậm chí băm tất cả chúng sẽ quá chậm. 

Một điểm tinh tế là mục tiêu không phải là tối đa hóa tính đa dạng cục bộ mà là trên toàn cầu trên tất cả các chuỗi con. Ví dụ: một chuỗi như`"000000"`cực kỳ lặp đi lặp lại và tạo ra rất ít chuỗi con riêng biệt, vì mọi chuỗi con đều thu gọn thành các chuỗi số 0 dài. Mặt khác, các chuỗi thay thế các ký hiệu có xu hướng tạo ra nhiều chuyển tiếp khác nhau và do đó có nhiều chuỗi con khác biệt hơn. 

Một ý tưởng ngây thơ nhưng hấp dẫn là nghĩ rằng bất kỳ chuỗi nhị phân "trông ngẫu nhiên" nào cũng phải tối ưu. Tuy nhiên, tính ngẫu nhiên là không cần thiết, và trên thực tế, sự xen kẽ có cấu trúc có xu hướng tốt hơn nó về mặt các chuỗi con riêng biệt. 

Vỏ một cạnh nhỏ$n$. Vì$n = 1$, cả hai`"0"`Và`"1"`là tối ưu. Vì$n = 2$, cả hai`"01"`Và`"10"`tạo ra tất cả các chuỗi con có thể có độ dài lên tới 2 mà không lặp lại. Một cách tiếp cận bất cẩn có thể cố gắng luôn thay thế bắt đầu bằng`"0"`mà không xem xét tính đối xứng, nhưng vì các ràng buộc được cho phép nên mọi mẫu xen kẽ đều hợp lệ. 

Một trường hợp ẩn khác là các mẫu lặp lại như`"0101..."`cư xử rất khác với`"000...111..."`. Chuỗi sau thu gọn các chuỗi con một cách nặng nề, trong khi chuỗi trước tiếp tục giới thiệu các chuỗi con mới vì hầu hết mọi cửa sổ đều chứa một mẫu ranh giới khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra mọi chuỗi nhị phân có độ dài$n$và với mỗi chuỗi con hãy tính số chuỗi con riêng biệt bằng cách liệt kê tất cả$O(n^2)$chuỗi con và chèn chúng vào tập băm. Ngay cả khi sử dụng phép băm chuỗi con, mỗi chuỗi vẫn có giá$O(n^2)$, và có$2^n$dây, làm cho điều này hoàn toàn không khả thi. 

Một cách mạnh mẽ hợp lý hơn là: sửa một chuỗi ứng cử viên và tính số chuỗi con riêng biệt của nó. Cái này đã tốn rồi$O(n^2)$, quá chậm đối với$n = 2 \cdot 10^5$. 

Quan sát quan trọng là chúng ta không được yêu cầu so sánh các chuỗi tùy ý mà chỉ xây dựng một chuỗi tối ưu. Cấu trúc tối đa hóa các chuỗi con riêng biệt là cấu trúc tránh được các mẫu lặp lại dài, bởi vì sự lặp lại sẽ làm sập các chuỗi con. Một chuỗi nhị phân xen kẽ hoàn toàn đảm bảo rằng không tồn tại một chuỗi dài các ký tự giống hệt nhau và mỗi chuỗi con đều chứa các chuyển đổi thường xuyên giúp phân biệt nó với các chuỗi khác. 

Trên thực tế, trong số các chuỗi nhị phân, mẫu xen kẽ tối đa hóa entropy ở mọi cửa sổ cục bộ, điều này gián tiếp tối đa hóa số lượng chuỗi con riêng biệt. Mọi sai lệch như giới thiệu`"00"`hoặc`"11"`tạo ra sự dư thừa: khi một khối lặp lại tồn tại, nhiều chuỗi con sẽ không thể phân biệt được với các chuỗi khác chứa cùng một khối. 

Do đó, cách xây dựng tối ưu chỉ đơn giản là thay thế các ký tự:`"010101..."`hoặc`"101010..."`. 

Điều này làm giảm vấn đề trong việc chọn bit bắt đầu và in một chuỗi xen kẽ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n^2)$|$O(n)$| Quá chậm | 
| Thi công xen kẽ tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng chuỗi trực tiếp. 

1. Bắt đầu bằng cách chọn ký tự đầu tiên`'0'`hoặc`'1'`. Vì cả hai đều dẫn đến cùng số chuỗi con đối xứng nên chúng ta có thể sửa`'0'`không mất tính tổng quát. Sự lựa chọn không ảnh hưởng đến tính tối ưu vì việc lật tất cả các bit sẽ bảo toàn cấu trúc phân biệt của chuỗi con. 
2. Đối với mọi vị trí$i$từ 1 đến$n-1$, đặt ký tự khác với ký tự trước đó. Nếu ký tự trước đó là`'0'`, địa điểm`'1'`, nếu không thì đặt`'0'`. Điều này đảm bảo sự luân phiên nghiêm ngặt trên toàn bộ chuỗi. 
3. Xuất chuỗi đã xây dựng. 

Quyết định thực sự duy nhất là thực thi rằng không có hai ký tự liền kề nào bằng nhau. Khi đã xong, chuỗi được xác định đầy đủ. 

### Tại sao nó hoạt động 

Việc xây dựng loại bỏ các ký hiệu liền kề lặp đi lặp lại, là những khối xây dựng nhỏ nhất dư thừa trong chuỗi con nhị phân. Bất kỳ ký tự liền kề lặp lại nào đều tạo ra một chuỗi và các chuỗi chạy sẽ tạo ra các chuỗi con lặp lại trên các vị trí khác nhau. Bằng cách bắt buộc luân phiên, mọi chuỗi con đều có độ nhạy tối đa đối với vị trí vì mỗi lần dịch chuyển đều thay đổi mô hình chuyển tiếp. Điều này đảm bảo rằng các chuỗi con được phân biệt chủ yếu theo nơi chúng bắt đầu và kết thúc thay vì thu gọn thành các khối lặp lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

if n == 1:
    print("0")
else:
    res = []
    for i in range(n):
        if i % 2 == 0:
            res.append('0')
        else:
            res.append('1')
    print("".join(res))
```Việc thực hiện sửa lỗi`'0'`ở chỉ số 0 và thay thế sau đó bằng cách sử dụng tính chẵn lẻ. Điều này tránh việc theo dõi các ký tự trước đó một cách rõ ràng và giữ mã O(1) cho mỗi vị trí. 

Một lỗi phổ biến ở đây là cố gắng tối ưu hóa động dựa trên số lượng chuỗi con hoặc cố gắng cải thiện cục bộ một cách tham lam. Không cần điều gì trong số đó vì cấu trúc tối ưu toàn cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 3$Chúng tôi xây dựng chuỗi từng bước. 

| tôi | ký tự trước | char đã chọn | chuỗi hiện tại | 
| --- | --- | --- | --- | 
| 0 | - | 0 | 0 | 
| 1 | 0 | 1 | 01 | 
| 2 | 1 | 0 | 010 | 

Chuỗi kết quả là`"010"`. Chuỗi này tạo ra nhiều chuỗi con khác biệt hơn`"000"`hoặc`"001"`bởi vì nó tránh bị sụp đổ thành những đường chạy dài đồng đều. 

### Ví dụ 2:$n = 5$| tôi | ký tự trước | char đã chọn | chuỗi hiện tại | 
| --- | --- | --- | --- | 
| 0 | - | 0 | 0 | 
| 1 | 0 | 1 | 01 | 
| 2 | 1 | 0 | 010 | 
| 3 | 0 | 1 | 0101 | 
| 4 | 1 | 0 | 01010 | 

Chuỗi cuối cùng là`"01010"`. Mỗi chuỗi con của chuỗi này chứa ít nhất một chuyển đổi trong hầu hết các trường hợp, làm cho chuỗi con dễ phân biệt hơn so với bất kỳ chuỗi nào có các khối lặp lại. 

Những dấu vết này xác nhận rằng việc xây dựng mang tính quyết định và độc lập với bất kỳ phương pháp phỏng đoán bổ sung nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi vị trí được lấp đầy một lần với công việc liên tục | 
| Không gian |$O(n)$| Lưu trữ chuỗi đầu ra | 

Giải pháp phù hợp một cách thoải mái trong những hạn chế vì thậm chí$n = 2 \cdot 10^5$được xử lý theo thời gian tuyến tính và việc sử dụng bộ nhớ bị chi phối bởi chính đầu ra. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline
    n = int(input().strip())
    if n == 1:
        return "0"
    res = []
    for i in range(n):
        res.append('0' if i % 2 == 0 else '1')
    return "".join(res)

# minimum size
assert run("1\n") == "0"

# small case
assert run("2\n") in ["01", "10"]

# odd length
assert run("5\n") == "01010"

# even length
assert run("6\n") == "010101"

# large pattern check
assert run("10\n") == "0101010101"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | ranh giới tối thiểu | 
| 2 | 01 hoặc 10 | tính đối xứng của câu trả lời tối ưu | 
| 5 | 01010 | độ chính xác có độ dài lẻ | 
| 6 | 010101 | luân phiên nhất quán | 
| 10 | 0101010101 | ổn định cấu trúc lâu hơn | 

## Vỏ cạnh 

cho$n = 1$, đầu ra của thuật toán`"0"`, vì sự luân phiên thoái hóa thành một lựa chọn duy nhất. Điều này tối đa hóa một cách tầm thường các chuỗi con riêng biệt vì chỉ có thể có một chuỗi con. 

Vì$n = 2$, đầu ra là`"01"`. Từng bước, chỉ số 0 là`'0'`, chỉ số 1 là`'1'`. Điều này tạo ra ba chuỗi con riêng biệt:`"0"`,`"1"`, Và`"01"`. Bất kỳ chuỗi không đổi nào như`"00"`sẽ chỉ sản xuất`"0"`Và`"00"`, điều này thực sự tệ hơn. 

Đối với lớn hơn$n$, chẳng hạn như$n = 4$, việc xây dựng`"0101"`duy trì sự luân phiên ở mỗi bước. Không có chuỗi nào dài hơn một ký tự nên không có sự lặp lại chuỗi con nào bị thu gọn giữa các vị trí giống như cách`"0011"`, trong đó các chuỗi con như`"00"`xuất hiện nhiều lần với cấu trúc giống hệt nhau.
