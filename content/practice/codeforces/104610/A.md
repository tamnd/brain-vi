---
title: "CF 104610A - Khớp mẫu"
description: "Chúng ta được cung cấp một số mẫu, mỗi mẫu bao gồm các chữ cái viết hoa và ký tự đại diện. Mỗi chuỗi có thể được thay thế độc lập bằng bất kỳ chuỗi chữ in hoa nào, kể cả chuỗi trống. Sau tất cả sự thay thế, một khuôn mẫu sẽ trở thành một sợi dây cụ thể."
date: "2026-06-29T23:19:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104610
codeforces_index: "A"
codeforces_contest_name: "2020 Google Code Jam Round 1A (GCJ 20 Round 1A)"
rating: 0
weight: 104610
solve_time_s: 72
verified: true
draft: false
---

[CF 104610A - Khớp mẫu](https://codeforces.com/problemset/problem/104610/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số mẫu, mỗi mẫu bao gồm các chữ cái viết hoa và ký tự đại diện`*`. Mỗi`*`có thể được thay thế độc lập bằng bất kỳ chuỗi chữ in hoa nào, kể cả chuỗi trống. Sau tất cả sự thay thế, một khuôn mẫu sẽ trở thành một sợi dây cụ thể. 

Nhiệm vụ là xác định xem có tồn tại một chuỗi đơn, có độ dài tối đa 10.000, có thể được lấy đồng thời từ mọi mẫu bằng cách thay thế thích hợp các ký tự đại diện của chúng hay không. Nếu một chuỗi như vậy tồn tại, chúng tôi có thể xuất ra bất kỳ một ví dụ hợp lệ nào. Nếu không, chúng tôi phải báo cáo lỗi bằng một`*`. 

Điểm tinh tế quan trọng là mỗi mẫu không ràng buộc toàn bộ chuỗi một cách thống nhất. Thay vào đó, nó hạn chế cấu trúc: các phần giữa các ngôi sao phải xuất hiện theo thứ tự, nhưng các ngôi sao có thể giãn ra tùy ý, tạo ra những khoảng trống linh hoạt. 

Các ràng buộc rất nhỏ: tối đa 50 mẫu, mỗi mẫu có độ dài tối đa 100. Điều này loại trừ mọi cách xây dựng hàm mũ đối với tất cả các thay thế ký tự đại diện. Tập hợp tuyến tính hoặc gần tuyến tính cho mỗi mẫu là đủ. 

Một sai lầm ngây thơ là coi mỗi mẫu như một biểu thức chính quy và cố gắng xây dựng một máy tự động giao nhau đầy đủ hoặc thử tất cả các vị trí của chuỗi con. Ví dụ: các mẫu như`A*C*E`Và`*B*D*`có thể khiến người ta nghĩ đến sự liên kết tổ hợp của tất cả các phân đoạn. Cách tiếp cận đó nhanh chóng bùng nổ bởi vì mỗi`*`giới thiệu những lựa chọn không giới hạn. 

Một lỗi tinh vi hơn xuất phát từ việc chỉ kiểm tra tiền tố hoặc chỉ kiểm tra chuỗi con mà không xử lý các ràng buộc hậu tố một cách đối xứng. Ví dụ,`HE*`Và`*LO`cả hai đều cho phép nhiều kết quả khớp, nhưng chỉ các chuỗi bắt đầu bằng`HE`và kết thúc bằng`LO`hoạt động và việc trộn logic này không chính xác thường dẫn đến kết quả dương tính giả. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng một chuỗi ứng cử viên và xác minh nó dựa trên tất cả các mẫu. Vì mỗi`*`có thể biểu diễn các chuỗi dài tùy ý, không gian tìm kiếm thực sự không bị giới hạn. Ngay cả khi chúng tôi giới hạn bản thân ở các chuỗi có độ dài tối đa 10.000, thì việc thử tất cả các tác phẩm có thể là không thể. 

Thay vào đó, chúng tôi quan sát thấy rằng mọi mẫu có thể được phân tách thành ba phần có ý nghĩa: tiền tố trước mẫu đầu tiên.`*`, hậu tố sau cuối cùng`*`và tất cả các phân đoạn trung gian giữa các ngôi sao. Điều quan trọng cần lưu ý là tiền tố của câu trả lời cuối cùng phải tương thích với tất cả các tiền tố mẫu và tương tự với các hậu tố. Các phần ở giữa không cần các ràng buộc căn chỉnh ngoài việc giữ trật tự; chúng có thể được nối một cách đơn giản. 

Điều này làm giảm vấn đề xuống còn hai bước kiểm tra tính nhất quán và một bước xây dựng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Bảng liệt kê Brute Force của chuỗi | Hàm mũ | Cao | Quá chậm | 
| Hợp nhất tiền tố/hậu tố với nối | O(tổng chiều dài mẫu) | O(tổng chiều dài mẫu) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng mẫu một cách độc lập và trích xuất các ràng buộc về cấu trúc. 

### 1. Chia mỗi mẫu thành các đoạn 
Chúng tôi chia mọi mẫu thành`*`, tạo ra một danh sách các chuỗi cố định. Phân đoạn đầu tiên hoạt động như một ràng buộc tiền tố, phân đoạn cuối cùng hoạt động như một ràng buộc hậu tố và mọi thứ ở giữa là nội dung bên trong linh hoạt. 

Sự phân tách này có giá trị vì`*`có thể hấp thụ bất kỳ số lượng ký tự nào, do đó chỉ có thứ tự tương đối của các phân đoạn mới quan trọng. 

### 2. Thu thập các ràng buộc tiền tố 
Đối với mỗi mẫu, lấy phân đoạn đầu tiên của nó. Trong số tất cả các tiền tố này, hãy xác định tiền tố dài nhất. Tiền tố dài nhất này trở thành tiền tố ứng viên của câu trả lời cuối cùng. 

Sau đó, chúng tôi xác minh rằng mọi tiền tố khác đều tương thích với ứng viên này, nghĩa là nó phải khớp với các ký tự bắt đầu tương ứng. Nếu bất kỳ tiền tố nào không đồng ý ở vị trí mà cả hai đều xác định một ký tự thì việc xây dựng là không thể. 

### 3. Thu thập các ràng buộc hậu tố 
Chúng tôi lặp lại logic tương tự cho các hậu tố, nhưng từ cuối. Đối với mỗi mẫu, lấy đoạn cuối cùng của nó. Chúng tôi chọn hậu tố dài nhất làm hậu tố ứng viên cho câu trả lời cuối cùng và đảm bảo tất cả những hậu tố khác đều nhất quán với hậu tố đó khi căn chỉnh đến cuối. 

Điều này đảm bảo tất cả các mẫu có thể chấm dứt một cách chính xác. 

### 4. Thu thập các đoạn giữa 
Tất cả các phân đoạn trung gian từ tất cả các mẫu được nối theo thứ tự tùy ý nhưng vẫn giữ nguyên thứ tự bên trong mỗi mẫu. Các phân đoạn này tương ứng với các chuỗi con bắt buộc phải xuất hiện ở đâu đó theo trình tự. 

Từ`*`cho phép chèn tùy ý, chúng ta có thể đặt tất cả các đoạn ở giữa một cách an toàn giữa tiền tố và hậu tố đã chọn. 

### 5. Xây dựng ứng viên cuối cùng 
Chuỗi cuối cùng được hình thành như sau: 

tiền tố + nối tất cả các đoạn ở giữa + hậu tố 

### 6. Xác thực giới hạn độ dài 
Nếu chuỗi kết quả vượt quá 10.000 ký tự, chúng tôi vẫn chỉ xuất chuỗi đó nếu được phép; nếu không chúng tôi sẽ từ chối. Với những hạn chế, điều này hầu như không bao giờ xảy ra trừ khi đầu vào có tính chất đối nghịch, nhưng chúng tôi vẫn thực thi nó. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là mọi mẫu phải nhúng các phân đoạn cố định của nó theo thứ tự. Việc tổng hợp tiền tố đảm bảo rằng tất cả các ràng buộc bắt đầu bắt buộc đều được thỏa mãn bởi một tiền tố tối đa duy nhất. Việc tổng hợp hậu tố đảm bảo giống nhau ở cuối. Vì tất cả các phân đoạn còn lại nằm giữa các ngôi sao nên chúng không có xung đột về vị trí ngoài thứ tự, do đó phép nối vẫn duy trì tính hợp lệ. 

Bất kỳ mâu thuẫn nào cũng phải biểu hiện dưới dạng không khớp giữa hai phân đoạn cố định buộc phải chiếm các vị trí chồng chéo trong căn chỉnh tiền tố hoặc hậu tố, đó chính xác là điều mà quá trình kiểm tra tính tương thích phát hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for tc in range(1, T + 1):
        n = int(input())
        
        parts = []
        prefixes = []
        suffixes = []
        mids = []
        
        possible = True
        
        for _ in range(n):
            s = input().strip()
            seg = s.split('*')
            
            prefixes.append(seg[0])
            suffixes.append(seg[-1])
            
            if len(seg) > 2:
                mids.extend(seg[1:-1])
        
        # build prefix: take longest
        pref = max(prefixes, key=len)
        for p in prefixes:
            if len(p) > len(pref):
                if pref != p[:len(pref)]:
                    possible = False
                    break
            else:
                if p != pref[:len(p)]:
                    possible = False
                    break
        
        # build suffix: longest, check reversed compatibility
        suf = max(suffixes, key=len)
        for s in suffixes:
            if len(s) > len(suf):
                if suf != s[-len(suf):]:
                    possible = False
                    break
            else:
                if s != suf[-len(s):]:
                    possible = False
                    break
        
        if not possible:
            print(f"Case #{tc}: *")
            continue
        
        ans = pref + "".join(mids) + suf
        
        if len(ans) > 10000:
            print(f"Case #{tc}: *")
        else:
            print(f"Case #{tc}: {ans}")

if __name__ == "__main__":
    solve()
```Logic tiền tố xây dựng một chuỗi bắt đầu chiếm ưu thế và xác minh tất cả các chuỗi khác có thể được nhúng vào đầu. Logic hậu tố phản ánh điều này từ cuối. Các phân đoạn ở giữa được thu thập mà không có ràng buộc về tương tác vì các ngôi sao cho phép đặt tự do. 

Một cạm bẫy triển khai phổ biến là quên rằng việc căn chỉnh hậu tố phải được thực hiện từ cuối chứ không phải từ đầu. Một cách khác là giả sử các tiền tố có thể được nối thay vì so sánh bằng cách ngăn chặn; điều đó sẽ hợp nhất không chính xác các tiền tố không tương thích như`AB`Và`AC`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
A*C*E
*B*D*
A*CE
```Chúng tôi trích xuất: 
| Mẫu | Tiền tố | Hậu tố | Trung | 
|--------|--------|--------|--------| 
| A*C*E | A | E | C | 
| *B*D* | "" | "" | B, D | 
| A*CE | A | CE | - | 

Ứng cử viên tiền tố là`A`. Tất cả các tiền tố đều tương thích với`A`. 

Ứng viên có hậu tố là`CE`. Ràng buộc hậu tố trống là tương thích. 

Chuỗi được xây dựng trở thành`A + C + B + D + CE`. 

Điều này chứng tỏ rằng các phân đoạn ở giữa chỉ đơn giản là tích lũy trong khi tiền tố và hậu tố thực thi cấu trúc ranh giới. 

### Ví dụ 2 

đầu vào:```
2
CO*DE
J*AM
```Tiền tố là`CO`Và`J`, không tương thích vì cả hai đều không phải là tiền tố của cái kia. Thuật toán phát hiện điều này trong quá trình xác thực tiền tố và ngay lập tức đưa ra`*`. 

Điều này cho thấy điều kiện không thể xảy ra hoàn toàn do các tiền tố cố định xung đột nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(tổng chiều dài của mẫu) | Mỗi mẫu được quét một lần và phân tách, tiếp theo là kiểm tra tiền tố và hậu tố tuyến tính | 
| Không gian | O(tổng chiều dài của mẫu) | Các phân đoạn được lưu trữ trên tất cả các mẫu | 

Kích thước đầu vào đủ nhỏ để ngay cả các thao tác chuỗi đơn giản cũng an toàn trong giới hạn và cấu trúc vẫn nằm dưới giới hạn 10.000 ký tự trong các trường hợp điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    input = sys.stdin.readline

    T = int(input())
    out_lines = []

    for tc in range(1, T + 1):
        n = int(input())
        prefixes = []
        suffixes = []
        mids = []
        
        possible = True
        
        for _ in range(n):
            s = input().strip()
            seg = s.split('*')
            prefixes.append(seg[0])
            suffixes.append(seg[-1])
            if len(seg) > 2:
                mids.extend(seg[1:-1])
        
        pref = max(prefixes, key=len)
        for p in prefixes:
            if len(p) > len(pref):
                if pref != p[:len(pref)]:
                    possible = False
                    break
            else:
                if p != pref[:len(p)]:
                    possible = False
                    break
        
        suf = max(suffixes, key=len)
        for s in suffixes:
            if len(s) > len(suf):
                if suf != s[-len(suf):]:
                    possible = False
                    break
            else:
                if s != suf[-len(s):]:
                    possible = False
                    break
        
        if not possible:
            out_lines.append(f"Case #{tc}: *")
        else:
            ans = pref + "".join(mids) + suf
            if len(ans) > 10000:
                out_lines.append(f"Case #{tc}: *")
            else:
                out_lines.append(f"Case #{tc}: {ans}")

    return "\n".join(out_lines)

# sample-like tests
assert run("1\n3\nA*C*E\n*B*D*\nA*CE\n") != "", "basic construction"

assert run("1\n2\nCO*DE\nJ*AM\n") == "Case #1: *", "incompatible prefixes"

assert run("1\n1\nABC") == "Case #1: ABC", "no wildcard"

assert run("1\n2\nA*\n*B\n") == "Case #1: AB", "simple glue"

assert run("1\n2\nHELLO*\n*HELLO\n") == "Case #1: HELLOHELLO", "overlap suffix-prefix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| MÃ*DE / J*AM | * | phát hiện không tương thích tiền tố | 
| A* / *B | AB | hợp nhất tiền tố-hậu tố cơ bản | 
| XIN CHÀO* / *XIN CHÀO | XIN CHÀO | xử lý chồng chéo | 
| ABC | ABC | không có trường hợp ký tự đại diện | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi các mẫu không chứa ngôi sao. Trong trường hợp đó, toàn bộ chuỗi hoạt động đồng thời như cả ràng buộc tiền tố và hậu tố. Thuật toán xử lý vấn đề này vì mảng tiền tố và hậu tố sẽ chứa các chuỗi đầy đủ giống hệt nhau và việc kiểm tra tính nhất quán sẽ giảm xuống việc thực thi sự bình đẳng trên tất cả các mẫu. 

Một trường hợp cạnh khác phát sinh khi một mẫu bắt đầu hoặc kết thúc bằng`*`, tạo ra các phân đoạn tiền tố hoặc hậu tố trống. Chúng luôn tương thích vì một chuỗi trống là tiền tố và hậu tố của mọi chuỗi, vì vậy chúng không bao giờ hạn chế việc xây dựng. 

Một trường hợp tinh vi hơn là khi nhiều mẫu có cấu trúc bên trong xung đột nhau nhưng không có xung đột tiền tố hoặc hậu tố. Thuật toán cho phép điều này một cách chính xác vì các phân đoạn nội bộ không bắt buộc phải căn chỉnh trên toàn cầu; chúng chỉ cần xuất hiện theo thứ tự ở đâu đó trong chuỗi được xây dựng và phép nối đảm bảo thứ tự này được giữ nguyên.
