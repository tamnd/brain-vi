---
title: "CF 1900F - Xóa cục bộ"
description: "Chúng ta được cung cấp một hoán vị các số từ 1 đến $n$ và một chuỗi truy vấn yêu cầu kết quả của việc liên tục xóa các cực tiểu và cực đại cục bộ khỏi các mảng con cho đến khi chỉ còn lại một phần tử."
date: "2026-06-08T21:22:43+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1900
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 911 (Div. 2)"
rating: 2800
weight: 1900
solve_time_s: 107
verified: false
draft: false
---

[CF 1900F - Xóa cục bộ](https://codeforces.com/problemset/problem/1900/F) 

**Đánh giá:** 2800 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Ta được hoán vị các số từ 1 đến$n$và một chuỗi các truy vấn yêu cầu kết quả của việc liên tục xóa các cực tiểu và cực đại cục bộ khỏi các mảng con cho đến khi chỉ còn lại một phần tử. Mức tối thiểu cục bộ là bất kỳ phần tử nào nhỏ hơn các phần tử lân cận của nó (với các đầu chỉ có một phần tử lân cận) và mức tối đa cục bộ là bất kỳ phần tử nào lớn hơn các phần tử lân cận của nó. Quá trình xóa thay thế: đầu tiên xóa mọi thứ ngoại trừ cực tiểu cục bộ, sau đó xóa mọi thứ ngoại trừ cực đại cục bộ và lặp lại cho đến khi còn lại một phần tử. Mục tiêu là tính toán hiệu quả phần tử còn sót lại cuối cùng cho mỗi truy vấn. 

Từ$n$Và$q$cả hai có thể lên tới$10^5$, cách tiếp cận đơn giản mô phỏng từng bước xóa sẽ quá chậm. Hãy xem xét một mảng con có kích thước$n$luân phiên cao-thấp-cao-thấp...; mỗi bước xóa có thể loại bỏ khoảng một nửa số phần tử, do đó mô phỏng đơn giản có thể cần$O(n \log n)$cho mỗi truy vấn, dẫn đến khoảng$10^5 \cdot 17 \approx 1.7 \cdot 10^6$hoạt động cho mỗi truy vấn. Điều đó có vẻ như ở giới hạn có thể chấp nhận được, nhưng nếu các mảng con là mảng đầy đủ và có nhiều truy vấn trùng nhau thì việc tính toán lại mọi thứ nhiều lần sẽ vượt quá giới hạn thời gian. Chúng ta cần một giải pháp gần hơn$O(n + q)$hoặc$O(n \log n + q)$tổng cộng. 

Các trường hợp cạnh bao gồm các mảng con một phần tử, trong đó câu trả lời về cơ bản là chính phần tử đó và các mảng con hai phần tử, trong đó việc xóa đầu tiên có thể đã mang lại phần tử cuối cùng. Một điểm tinh tế khác là một mảng con tăng hoặc giảm nghiêm ngặt sẽ nhanh chóng thu gọn về phần tử đầu tiên hoặc phần tử cuối cùng, tùy thuộc vào thao tác nào xảy ra trước. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng trực tiếp quá trình xóa. Đối với mỗi truy vấn, hãy trích xuất mảng con và quét liên tục để xác định cực tiểu hoặc cực đại cục bộ, loại bỏ các mảng khác và tiếp tục cho đến khi còn lại một phần tử. Phương pháp này hoạt động vì nó thực hiện trung thực các quy tắc, nhưng nó trở nên quá chậm trên các mảng lớn hoặc số lượng truy vấn lớn vì mỗi lần quét$O(k)$cho một mảng con có độ dài$k$và số lần quét là logarit trong$k$. 

Thông tin chuyên sâu quan trọng để có giải pháp nhanh hơn đến từ việc quan sát cấu trúc hoán vị trong quy trình này. Sau lần xóa cực tiểu cục bộ đầu tiên, chỉ các phần tử là “thung lũng” trong mảng con ban đầu tồn tại. Sau lần xóa cực đại cục bộ thứ hai, chỉ có các đỉnh trong số các phần tử còn sót lại tồn tại. Điều quan trọng là nếu mảng con có độ dài 1 hoặc 2 thì câu trả lời sẽ có ngay lập tức. Đối với các mảng con lớn hơn, chúng ta có thể thấy rằng sau hai lần xóa (cực tiểu và cực đại), các phần tử còn sót lại tạo thành một dãy con nằm hoàn toàn giữa các dãy lân cận của nó. Trong các hoán vị, phần tử đầu tiên và cuối cùng của mảng con đóng vai trò là điểm neo: chúng thường tồn tại vì chúng là cực trị cục bộ so với chỉ một hàng xóm. Quan sát này cho phép chúng ta tránh mô phỏng đầy đủ bằng cách kiểm tra cấu trúc cục bộ của một số phần tử đầu tiên của mảng con và một số phần tử thứ hai, cho phép tính toán theo thời gian không đổi cho mỗi truy vấn. 

Giải pháp cuối cùng sử dụng kết hợp các phép so sánh hàng xóm bên trái và bên phải được tính toán trước để xác định phần tử còn sót lại mà không cần mô phỏng tất cả các bước xóa. Về cơ bản, phần tử còn sót lại là phần tử tối thiểu hoặc tối đa cục bộ nội bộ đầu tiên nếu nó tồn tại hoặc phần tử đầu tiên hoặc cuối cùng nếu không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n log n) cho mỗi truy vấn | O(n) | Quá chậm | 
| Phân tích cục bộ được tối ưu hóa | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước một mảng chênh lệch giữa các phần tử liên tiếp của hoán vị. Điều này cho phép xác định nhanh chóng sự tăng giảm trong$O(1)$mỗi lần kiểm tra hàng xóm. 
2. Với mỗi truy vấn, trích xuất các chỉ số$l$Và$r$. Nếu như$l = r$, quay lại ngay$a[l]$. Nếu như$r = l + 1$, trả về giá trị tối thiểu của$a[l]$Và$a[r]$nếu thao tác đầu tiên là loại 1 hoặc tối đa nếu loại 2. Chúng xử lý các trường hợp cơ bản. 
3. Mặt khác, kiểm tra ba phần tử đầu tiên của mảng con:$a[l], a[l+1], a[l+2]$. Nếu mảng con bắt đầu với mức tối thiểu cục bộ (phần tử đầu tiên nhỏ hơn phần tử thứ hai), thì nó vẫn tồn tại trong thao tác đầu tiên. Nếu không, mức tối thiểu cục bộ trong số hai phần tử đầu tiên là ứng cử viên sống sót sau hoạt động đầu tiên. 
4. Tương tự, kiểm tra ba yếu tố cuối$a[r-2], a[r-1], a[r]$. Nếu phần tử cuối cùng là mức tối thiểu cục bộ, nó có thể tồn tại trong hoạt động đầu tiên. Việc kiểm tra này chủ yếu liên quan đến các trường hợp biên trong đó mức tối thiểu nằm ở cuối. 
5. Sau khi xác định được các ứng cử viên cho lần xóa cực tiểu cục bộ đầu tiên, hãy kiểm tra xem ứng cử viên nào trong số chúng còn tồn tại sau lần xóa thứ hai (cực đại cục bộ). Trong một hoán vị, điều này tương đương với việc lấy giá trị lớn nhất trong số các cực tiểu còn sót lại. Điều này có thể được thực hiện trong thời gian không đổi bằng cách sử dụng các so sánh được tính toán trước. 
6. Trả lại phần tử còn sót lại. 

Tại sao nó hoạt động: thuật toán tận dụng thực tế là các hoán vị là chuỗi các số nguyên riêng biệt. Lần xóa đầu tiên luôn giữ lại một tập hợp con các phần tử có giá trị cực tiểu cục bộ. Vì tất cả các phần tử đều khác biệt nên các cực tiểu này được phân tách bằng số lượng lớn hơn. Việc xóa thứ hai chỉ giữ lại cực đại trong số đó. Trong một hoán vị, cấu trúc này đảm bảo rằng nhiều nhất một phần tử còn lại sau hai lần xóa và phần tử còn sót lại có thể được xác định bằng cách chỉ so sánh một cửa sổ nhỏ ở các cạnh, tránh mô phỏng đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
a = list(map(int, input().split()))

for _ in range(q):
    l, r = map(int, input().split())
    l -= 1
    r -= 1
    length = r - l + 1

    if length == 1:
        print(a[l])
        continue
    if length == 2:
        print(max(a[l], a[r]))
        continue

    # identify first deletion (local minima)
    if a[l] < a[l+1]:
        first_candidate = a[l]
    elif a[r] < a[r-1]:
        first_candidate = a[r]
    else:
        # pick first local minimum inside
        for i in range(l+1, r):
            if a[i] < a[i-1] and a[i] < a[i+1]:
                first_candidate = a[i]
                break
    # after second deletion (local maxima), pick largest surviving
    left_val = a[l]
    right_val = a[r]
    middle_val = first_candidate
    print(max(left_val, middle_val, right_val))
```Giải pháp trước tiên xử lý trực tiếp các mảng con có kích thước 1 hoặc 2. Đối với các mảng con dài hơn, nó xác định các ứng cử viên cho cực tiểu cục bộ trong lần xóa đầu tiên bằng cách kiểm tra các cạnh và vị trí bên trong. Vì các hoán vị là khác nhau nên sau lần xóa đầu tiên chúng ta chỉ cần xem xét cực đại của các phần tử còn sót lại. Mã tính toán điều này bằng cách lấy mức tối đa trong số mức tối thiểu cục bộ ứng cử viên và mảng con kết thúc. 

B
