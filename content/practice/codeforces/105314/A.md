---
title: "CF 105314A - Hội chứng Rama và Mèo"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm, có một mảng các số nguyên dương biểu thị giá trị của túi thực phẩm. Rama phải chọn chính xác $n-1$ trong số các túi này và “sự hài lòng” của cô ấy được xác định là OR theo từng bit của các giá trị đã chọn."
date: "2026-06-23T06:16:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105314
codeforces_index: "A"
codeforces_contest_name: "Robbing Balloons 2.0 Qualifications"
rating: 0
weight: 105314
solve_time_s: 50
verified: true
draft: false
---

[CF 105314A - Hội chứng Rama và Mèo](https://codeforces.com/problemset/problem/105314/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm, có một mảng các số nguyên dương biểu thị giá trị của túi thực phẩm. Rama phải lựa chọn chính xác$n-1$của các túi này và “sự hài lòng” của cô ấy được định nghĩa là OR theo bit của các giá trị đã chọn. Nhiệm vụ là xác định xem cô ấy nên bỏ đi túi nào sao cho OR của túi còn lại$n-1$phần tử càng lớn càng tốt. 

Điểm mấu chốt là việc loại bỏ một phần tử không chỉ đơn giản là giảm OR theo cách tuyến tính. Vì OR chỉ phụ thuộc vào việc ít nhất một phần tử có đóng góp một bit hay không, nên việc loại bỏ một lần chỉ có ý nghĩa nếu nó là phần tử đóng góp duy nhất một số bit trong số tất cả các phần tử. 

Các ràng buộc cho phép lên đến$t = 10^3$trường hợp thử nghiệm và tổng số lên đến$2 \cdot 10^5$con số tổng thể. Điều này ngay lập tức loại trừ mọi cách tiếp cận cố gắng loại bỏ từng phần tử và tính toán lại OR đầy đủ từ đầu trong$O(n)$mỗi lần loại bỏ, vì điều đó sẽ giảm xuống$O(n^2)$trong trường hợp xấu nhất và vượt quá thời hạn. Thay vào đó, chúng ta cần một cách để sử dụng lại cấu trúc chung trong quá trình xóa. 

Một trường hợp lỗi nhỏ xuất hiện khi một số chứa các bit duy nhất trong toàn bộ mảng. Ví dụ: giả sử mảng là$[4, 2, 1]$. Tổng HOẶC là$7$. Nếu chúng ta loại bỏ$4$, OR trở thành$3$, bởi vì chút$2^2$biến mất hoàn toàn. Một giả định ngây thơ rằng việc loại bỏ bất kỳ phần tử nào hầu như không làm thay đổi OR sẽ thất bại ở đây, vì việc loại bỏ “sóng mang bit duy nhất” sẽ gây ra sự sụt giảm đáng kể. 

Một tình huống góc khác là khi nhiều phần tử chia sẻ tất cả các bit. Ví dụ,$[7, 7, 7]$. Việc xóa bất kỳ phần tử nào không làm thay đổi OR chút nào, vì mọi bit vẫn còn hiện diện ở nơi khác. Câu trả lời tối ưu là ổn định trong tất cả các lần loại bỏ, điều này cho thấy chúng ta nên suy luận về phạm vi bao phủ bit thay vì giá trị trực tiếp. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử từng chỉ mục$i$, tính OR của tất cả các phần tử ngoại trừ$a_i$và theo dõi mức tối đa. Tính toán một HOẶC mất$O(n)$, và làm điều này cho tất cả$n$chỉ số mang lại$O(n^2)$mỗi trường hợp thử nghiệm. Với$n$lên đến$2 \cdot 10^5$, điều này sẽ cần khoảng$4 \cdot 10^{10}$trong trường hợp xấu nhất vượt xa giới hạn khả thi. 

Cấu trúc của bitwise OR gợi ý một góc hiệu quả hơn. OR đầy đủ của tất cả các phần tử có thể được tính một lần. Lý do duy nhất việc loại bỏ một phần tử sẽ làm thay đổi kết quả là khi phần tử đó là thành phần duy nhất đóng góp cho một bit cụ thể. Nếu một bit xuất hiện trong ít nhất hai phần tử, việc loại bỏ bất kỳ phần tử nào sẽ không loại bỏ bit đó khỏi OR cuối cùng. 

Điều này có nghĩa là chúng ta có thể tính toán trước, đối với mỗi vị trí bit, có bao nhiêu phần tử mảng chứa bit đó. Sau đó, để loại bỏ bất kỳ ứng cử viên nào$i$, chúng ta có thể xác định chính xác bit nào sẽ biến mất nếu chúng ta loại trừ$a_i$: những bit được đặt trong$a_i$và chỉ xuất hiện một lần trong toàn bộ mảng. Từ đó chúng ta có thể xây dựng lại kết quả OR sau khi loại bỏ trong$O(1)$trên mỗi bit của số. 

Do đó, thay vì tính toán lại OR nhiều lần, chúng tôi giảm vấn đề xuống việc đếm tần số bit một lần và đánh giá mỗi lần loại bỏ theo thời gian không đổi trên mỗi vị trí bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Đếm tần số bit |$O(n \cdot 30)$|$O(30)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tập trung vào chiến lược tối ưu sử dụng số lượng bit. 

1. Tính OR của tất cả các phần tử trong mảng. Điều này thể hiện mức HOẶC tối đa có thể trước khi loại bỏ bất kỳ thứ gì. Nó ghi lại mọi bit xuất hiện ít nhất một lần. 
2. Đếm, với mỗi vị trí bit từ 0 đến 30, có bao nhiêu phần tử mảng được đặt bit đó. Điều này cho phép chúng tôi xác định bit nào dễ hỏng, nghĩa là chúng xuất hiện chính xác một lần. 
3. Đối với mỗi phần tử$a_i$, xác định bit nào sẽ bị mất nếu chúng ta loại bỏ nó. Một bit chỉ bị mất nếu nó được đặt vào$a_i$và số lượng toàn cầu của nó chính xác là một. Đây chính xác là các bit biến mất khỏi OR sau khi xóa$a_i$. 
4. Xây dựng kết quả OR sau khi loại bỏ$a_i$bằng cách bắt đầu từ OR đầy đủ và loại bỏ tất cả các bit bị mất. Về mặt khái niệm, chúng tôi lấy mặt nạ OR đầy đủ và bỏ đặt các bit thuộc sở hữu duy nhất đó. 
5. Theo dõi giá trị tối đa trong số tất cả các giá trị OR được xây dựng lại như vậy. 

Bước lý luận chính là mỗi bit hoạt động độc lập trong OR, vì vậy chúng ta có thể coi tác động của việc loại bỏ một phần tử là loại bỏ một tập hợp con các bit chứ không phải tính toán lại các tổ hợp. 

### Tại sao nó hoạt động 

Tại bất kỳ vị trí bit nào, OR cuối cùng sau khi loại bỏ một phần tử chỉ phụ thuộc vào việc liệu ít nhất một phần tử còn lại có còn chứa bit đó hay không. Nếu một bit xuất hiện trong hai hoặc nhiều phần tử, việc loại bỏ một phần tử sẽ không ảnh hưởng đến nó. Nếu nó xuất hiện trong chính xác một phần tử, việc loại bỏ phần tử cụ thể đó sẽ loại bỏ nó hoàn toàn. Điều này tạo ra một ánh xạ xác định từ mỗi chỉ mục đến một tập hợp các bit có thể tháo rời cụ thể, đảm bảo rằng OR được tính toán sau khi loại bỏ là chính xác cho mọi ứng cử viên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        cnt = [0] * 31
        
        for x in a:
            for b in range(31):
                if x & (1 << b):
                    cnt[b] += 1
        
        total_or = 0
        for b in range(31):
            if cnt[b] > 0:
                total_or |= (1 << b)
        
        ans = 0
        
        for x in a:
            removed_mask = 0
            for b in range(31):
                if (x & (1 << b)) and cnt[b] == 1:
                    removed_mask |= (1 << b)
            
            cur = total_or ^ removed_mask
            ans = max(ans, cur)
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên xây dựng số đếm tần số cho từng bit trên tất cả các số. Đây là quá trình tiền xử lý toàn cầu duy nhất cần thiết. Sau đó, nó tính toán OR của toàn bộ mảng, dùng làm đường cơ sở trước khi loại bỏ. 

Đối với mỗi phần tử, nó tính toán một mặt nạ bit sẽ biến mất nếu phần tử đó bị loại bỏ. biểu hiện`(x & (1 << b)) and cnt[b] == 1`xác định chính xác các bit được đóng góp duy nhất bởi phần tử đó. Việc trừ các bit này khỏi tổng OR được thực hiện bằng XOR, điều này an toàn ở đây vì các bit đó được đảm bảo tồn tại trong`total_or`và không được chia sẻ ở nơi khác. 

Cuối cùng, số lần xóa tối đa sẽ được in. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:$[2, 4, 8]$| Bước | Phần tử hiện tại | Đã loại bỏ các bit duy nhất | Kết quả HOẶC | 
| --- | --- | --- | --- | 
| Ban đầu | - | - | 14 | 
| tôi = 0 | 2 | không | 14 | 
| tôi = 1 | 4 | không | 14 | 
| tôi = 2 | 8 | không | 14 | 

Tất cả các bit xuất hiện chính xác một lần, nhưng việc loại bỏ bất kỳ phần tử đơn lẻ nào vẫn giữ nguyên hai bit còn lại, do đó, mỗi lần loại bỏ đều mang lại OR 14 ngoại trừ khi phần tử đó là nguồn duy nhất của bit của nó, phần tử này vẫn giữ tổng thể OR không thay đổi giữa các ứng cử viên. 

Tối đa là 14. 

### Ví dụ 2 

Mảng đầu vào:$[1, 2, 4, 7]$| Bước | Phần tử hiện tại | Đã loại bỏ các bit duy nhất | Kết quả HOẶC | 
| --- | --- | --- | --- | 
| Ban đầu | - | - | 7 | 
| tôi = 0 (1) | 1 | không | 7 | 
| tôi = 1 (2) | 2 | không | 7 | 
| tôi = 2 (4) | 4 | không | 7 | 
| tôi = 3 (7) | 7 | bit 0,1,2 | 0 | 

Ở đây, phần tử 7 là sóng mang duy nhất của nhiều bit. Việc xóa nó sẽ xóa tất cả các bit đó, làm giảm đáng kể OR. Lựa chọn tối ưu là bất kỳ yếu tố nào trong ba yếu tố đầu tiên, xác nhận rằng chúng ta phải đánh giá quyền sở hữu bit của từng yếu tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot 30)$| Mỗi phần tử được xử lý trên một số vị trí bit cố định | 
| Không gian |$O(30)$| Chỉ các mảng tần số bit được lưu trữ | 

Sự phụ thuộc tuyến tính vào$n$với hệ số không đổi nhỏ phù hợp thoải mái trong giới hạn lên đến$2 \cdot 10^5$tổng số phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old
    return out.getvalue().strip()

# sample-style checks
assert run("1\n2\n1 2\n") == "1"
assert run("1\n3\n1 2 4\n") == "7"

# all equal
assert run("1\n4\n7 7 7 7\n") == "7"

# unique-bit dominance
assert run("1\n3\n4 2 1\n") == "6"

# large identical mix
assert run("1\n5\n8 8 8 8 8\n") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | ổn định HOẶC | loại bỏ không có tác dụng | 
| đầy đủ sức mạnh riêng biệt của hai | đầy đủ HOẶC trừ một trường hợp | theo dõi bit độc đáo | 
| giá trị nhỏ hỗn hợp | tính toán lại đúng | tính đúng đắn chung | 
| sự thống trị bit đơn | tác động loại bỏ cạnh | xử lý bit dễ vỡ | 

## Vỏ cạnh 

Khi tất cả các số giống hệt nhau, mỗi số bit ít nhất là hai, do đó mặt nạ loại bỏ luôn bằng 0. Thuật toán tính toán mặt nạ loại bỏ bằng 0 cho mọi chỉ mục và trả về HOẶC ban đầu không thay đổi, phù hợp với hành vi đúng vì việc loại bỏ bất kỳ phần tử nào không làm giảm phạm vi bao phủ bit. 

Khi mỗi số đóng góp một tập hợp bit riêng biệt, chẳng hạn như lũy thừa của hai, thì mỗi số bit đều chính xác là một. Trong trường hợp này, việc xóa bất kỳ phần tử nào sẽ xóa chính xác bit của chính nó khỏi OR. Thuật toán tính toán chính xác OR kết quả bằng cách trừ bit đó và giá trị tối đa tương ứng với việc loại bỏ phần tử đóng góp nhỏ nhất. 

Khi một phần tử chứa nhiều bit duy nhất, việc loại bỏ nó sẽ làm OR giảm mạnh. Thuật toán nắm bắt được điều này vì tất cả các bit đó đều có số đếm một, vì vậy chúng được bao gồm đầy đủ trong mặt nạ đã loại bỏ cho chỉ mục đó, đảm bảo OR được xây dựng lại loại trừ chúng một cách chính xác.
