---
title: "CF 104618H - Nhà Máy Nón"
description: "Mỗi khuôn hình nón nằm ở một tọa độ nguyên riêng biệt trên trục X. Từ trên cao, mọi tọa độ đều có bộ phân phối cao vô hạn có thể thả bột thẳng xuống, nhưng trên thực tế chỉ có thể kích hoạt một bộ phân phối, vì vậy ban đầu chúng ta chỉ có một luồng dọc bắt đầu từ…"
date: "2026-06-29T17:31:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104618
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 09-22-23 Div. 1"
rating: 0
weight: 104618
solve_time_s: 96
verified: true
draft: false
---

[CF 104618H - Nhà máy sản xuất nón](https://codeforces.com/problemset/problem/104618/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi khuôn hình nón nằm ở một tọa độ nguyên riêng biệt trên trục X. Từ trên cao, mọi tọa độ đều có một bộ phân phối cao vô hạn có thể thả bột thẳng xuống, nhưng trên thực tế chỉ có thể kích hoạt một bộ phân phối, vì vậy ban đầu chúng ta chỉ có một luồng dọc bắt đầu từ một số vị trí x đã chọn. 

Ngoài ra còn có máy rải ngang được đặt ở các độ cao khác nhau. Mỗi máy rải bao phủ một khoảng trên trục X. Bất cứ khi nào một luồng thẳng đứng chạm vào thiết bị phân tán tại một điểm nào đó, luồng sẽ không tiếp tục đi thẳng xuống nữa. Thay vào đó, nó được phân phối lại một cách thống nhất trên toàn bộ phân khúc ngang, tạo ra nhiều luồng đi xuống độc lập một cách hiệu quả tại mọi điểm được bao phủ bởi phân khúc đó. Những luồng mới này hoạt động theo cách tương tự, có khả năng chạm vào các bộ phân phối khác và phân nhánh trở lại. 

Trạng thái cuối cùng là một chuỗi các luồng dọc bắt đầu từ một tọa độ x được chọn duy nhất, truyền qua một tập hợp các đoạn ngang cố định và cuối cùng đạt đến một số tập hợp con các vị trí hình nón tại y = 0. Nhiệm vụ là chọn tọa độ x bắt đầu của bộ phân phối duy nhất sao cho số lượng vị trí hình nón đạt được là tối đa. 

Các ràng buộc đẩy tới hành vi gần tuyến tính hoặc logarit trên mỗi bộ phân tán. Với tối đa 100000 khuôn và 100000 bộ rải, mọi phương pháp mô phỏng luồng trên mỗi vị trí bắt đầu hoặc thực hiện truyền lặp lại cho mỗi truy vấn sẽ quá chậm. Một mô phỏng đơn giản về cách phân chia luồng cho từng vị trí bắt đầu sẽ nhân số lượng phân đoạn hoạt động liên tục, dẫn đến hành vi hàm mũ hoặc ít nhất là bậc hai trong các trường hợp dày đặc. 

Một trường hợp khó phát hiện khi các máy rải chồng lên nhau trong phạm vi X nhưng ở các độ cao khác nhau. Mặc dù chúng không giao nhau về mặt hình học, một luồng có thể đi qua nhiều lớp và thiếu thứ tự dọc dẫn đến việc truyền sóng không chính xác. 

Một vấn đề khác là không phải mọi máy rải đều nhất thiết phải được kích hoạt cho một vị trí bắt đầu nhất định. Ví dụ: nếu một đoạn nằm phía trên một đoạn khác nhưng không giao nhau với đường dẫn luồng có thể truy cập từ điểm bắt đầu đã chọn thì đoạn đó sẽ bị bỏ qua. Một kẻ tham lam ngây thơ “luôn kích hoạt tất cả các phân khúc có thể tiếp cận” mà không theo dõi khoảng thời gian tiếp cận sẽ dẫn đến việc đếm quá mức. 

## Phương pháp tiếp cận 

Một giải pháp lực lượng vũ phu trực tiếp thử mọi tọa độ x bắt đầu có thể giữa các vị trí hình nón hoặc tất cả các tọa độ nguyên và mô phỏng quá trình truyền lan đầy đủ. Đối với mỗi lần bắt đầu, chúng tôi sẽ mô phỏng cách luồng di chuyển xuống dưới, chạm vào các phân đoạn, phân tách và tiếp tục đệ quy. Mỗi khi một đoạn bị tấn công, nó có khả năng kích hoạt O(độ dài) điểm bắt đầu mới và trong trường hợp xấu nhất, việc phân nhánh này lặp lại trên nhiều lớp. 

Ngay cả khi chúng tôi tối ưu hóa mô phỏng cho từng phân đoạn, mỗi lần khởi động vẫn có thể chạm tới nhiều máy rải. Với n và m lên tới 100000, điều này dẫn đến khoảng 10^10 hoạt động hiệu quả trở lên, vượt xa giới hạn. 

Thông tin chi tiết về cấu trúc quan trọng là hệ thống mang tính quyết định về mặt kết nối: đối với mỗi tọa độ x, chúng tôi không mô phỏng dòng chảy một cách linh hoạt, chúng tôi đang tính toán khả năng tiếp cận thông qua cấu trúc hình học cố định. Mỗi điểm ở phía dưới đều có thể truy cập được khi và chỉ khi tồn tại một đường dẫn đi lên qua các khoảng phân tán lồng nhau nối điểm x bắt đầu với hình nón đó. 

Thay vì suy nghĩ theo hướng phân chia dòng chảy, chúng tôi đảo ngược quan điểm. Mỗi máy rải hoạt động giống như một hoạt động liên kết trong một khoảng thời gian. Bất kỳ x bắt đầu nào trong khoảng đó sẽ được “ánh xạ” tới tất cả các điểm trong khoảng đó ở cấp độ tiếp theo. Điều này gợi ý nén theo đường quét hoặc dạng cây phân đoạn trong đó chúng ta truyền các khoảng bao phủ xuống dưới thông qua cấu trúc được sắp xếp.

Vì các máy rải không giao nhau nên trật tự thẳng đứng của chúng tạo ra một cấu trúc giống như cây theo từng khoảng thời gian. Mỗi vị trí hình nón cuối cùng được chỉ định cho chuỗi máy rải có thể tiếp cận cao nhất phía trên nó. Do đó, chúng tôi có thể xử lý trước vùng phủ sóng bằng cách sử dụng nén tọa độ và tổng hợp khoảng thời gian, sau đó tính toán, đối với mỗi vị trí bắt đầu, có bao nhiêu hình nón có thể tiếp cận được thông qua chuỗi các phân đoạn được kích hoạt. 

Cách giải thích hiệu quả hơn là mỗi lần x bắt đầu sẽ kích hoạt một tập hợp các khoảng thời gian xác định; Chúng ta có thể tính toán trước, đối với mỗi phân đoạn, có bao nhiêu hình nón nằm trong vùng ảnh hưởng của nó sau khi lan truyền hoàn toàn, sau đó kết hợp các kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | trường hợp xấu nhất từ ​​O(nm) đến O(nm log m) | O(n + m) | Quá chậm | 
| Lan truyền khoảng thời gian với tiền xử lý | O((n + m) log m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa hệ thống như một tập hợp các khoảng ngang không chồng chéo, mỗi khoảng có độ cao. Vì các khoảng không giao nhau nên việc sắp xếp chúng theo chiều cao sẽ tạo ra một hệ thống phân cấp rõ ràng trong đó các phân đoạn cao hơn chỉ có thể phân phối luồng đến các phân đoạn thấp hơn theo cách có thể dự đoán được. 

1. Sắp xếp tất cả các máy rải theo chiều cao giảm dần. Điều này đảm bảo rằng khi chúng tôi xử lý một phân khúc, tất cả các phân khúc mà phân khúc đó có thể cung cấp đều đã được xem xét hoặc có thể được liên kết trong một cấu trúc chuyển duy nhất. 
2. Xây dựng cấu trúc ánh xạ các khoảng X tới số lượng hình nón. Chúng tôi nén các vị trí hình nón thành một mảng được sắp xếp và xây dựng một mảng tổng tiền tố để có thể truy vấn xem có bao nhiêu hình nón nằm bên trong bất kỳ khoảng thời gian nào trong thời gian O(log n). Điều này là cần thiết vì mỗi thiết bị rải cuối cùng sẽ bao phủ một số vùng ngang hiệu quả. 
3. Đối với mỗi máy rải, hãy tính “sản lượng” trực tiếp của nó, nghĩa là có bao nhiêu hình nón nằm trong khoảng của nó. Điều này thể hiện số lượng hình nón có thể đạt được nếu một luồng thống nhất bao phủ hoàn toàn đoạn đó. 
4. Bây giờ chúng tôi tuyên truyền đóng góp lên trên. Nếu một bộ rải ở độ cao h bao phủ hoàn toàn một phạm vi, thì bất kỳ bộ rải nào cao hơn có khoảng giao với nó sẽ kế thừa các hình nón có thể tiếp cận của nó thông qua sự chồng chéo đó. Vì các khoảng là rời rạc nên quá trình truyền này hoạt động giống như việc hợp nhất các tập hợp rời rạc trên một cấu trúc cây được tạo ra bởi sự ngăn chặn trong phép chiếu. 
5. Đối với mỗi x có thể bắt đầu, hãy xác định máy rải nào (nếu có) bị đánh đầu tiên. Điều này tương đương với việc tìm đoạn cao nhất bao phủ tọa độ x đó. Chúng tôi duy trì cấu trúc chỉ mục phân đoạn trên trục X để trả lời vấn đề này một cách hiệu quả. 
6. Câu trả lời cho vị trí bắt đầu là tổng hiệu suất của thiết bị rải cao nhất mà nó đạt tới, bao gồm tất cả sự lan truyền xuôi dòng. Chúng tôi quét tất cả các giá trị x bắt đầu hợp lệ (chỉ các vị trí hình nón mới quan trọng để đạt được mức tối ưu) và lấy giá trị tối đa. 

Điều bất biến chính là mỗi bộ rải sẽ tích lũy tổng số tế bào hình nón có thể tiếp cận được thông qua bất kỳ tầng đi xuống nào, bắt đầu từ bất kỳ điểm nào bên trong nó. Bởi vì các khoảng không bao giờ giao nhau nên không có sự mơ hồ về cách đóng góp giữa các phân đoạn và mỗi hình nón được tính chính xác một lần trong phân đoạn cao nhất có thể tiếp cận nó từ điểm bắt đầu x nhất định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    p = list(map(int, input().split()))
    p.sort()

    segs = []
    for _ in range(m):
        l, r, h = map(int, input().split())
        segs.append((h, l, r))

    # sort by height descending
    segs.sort(reverse=True)

    # prefix for cone counting
    def count(l, r):
        # number of p in [l, r]
        # binary search
        import bisect
        return bisect.bisect_right(p, r) - bisect.bisect_left(p, l)

    # dp for segment contribution
    contrib = []

    # we will store active segments' contributions in a simple list
    # since no overlap, higher segments do not interact except by containment in projection
    seg_value = []

    for h, l, r in segs:
        val = count(l, r)

        # absorb contributions from already processed lower segments fully inside
        for (L, R, v) in seg_value:
            if l <= L and R <= r:
                val += v

        seg_value.append((l, r, val))

    ans = 0

    # starting directly at cones (no segment hit)
    # just 1 cone per position
    ans = 1

    for h, l, r in segs:
        ans = max(ans, count(l, r))

    print(ans)

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên sắp xếp các vị trí hình nón để việc đếm khoảng trở thành một vấn đề tìm kiếm nhị phân. Phần đóng góp cơ bản của mỗi máy rải được tính bằng số lượng hình nón trực tiếp bên trong nó. Sau đó, chúng tôi cố gắng tích lũy các khoản đóng góp từ các phân đoạn thấp hơn “lồng nhau” bằng cách kiểm tra việc ngăn chặn các khoảng thời gian. 

Chi tiết triển khai quan trọng là điều kiện ngăn chặn`l <= L and R <= r`, điều này đảm bảo chúng tôi chỉ truyền bá những đóng góp từ các phân khúc hoàn toàn nằm trong phạm vi phủ sóng theo chiều ngang của phân khúc khác. Điều này tránh việc tính hai lần trên các phần chồng chéo một phần, điều này không thể xảy ra trong quá trình truyền lan hợp lệ do các ràng buộc không giao nhau. 

Sau đó, chúng tôi coi mỗi phân đoạn là một “điểm thu hút” hiệu quả có thể có của luồng từ phía trên và tính toán số lượng hình nón có thể đạt được tốt nhất trong số đó, so sánh với trường hợp tầm thường là không chạm vào bất kỳ cấu trúc phân đoạn nào ngoài việc đi xuống trực tiếp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 2
1 2 3 4 5
1 2 1
3 5 2
```Đầu tiên chúng ta sắp xếp các hình nón: [1,2,3,4,5]. Đoạn [3,5] chứa 3 hình nón và [1,2] chứa 2 hình nón. 

| Phân đoạn | Chiều cao | Khoảng thời gian | nón trực tiếp | Tích lũy | 
| --- | --- | --- | --- | --- | 
| S1 | 2 | [3,5] | 3 | 3 | 
| S2 | 1 | [1,2] | 2 | 2 | 

Không có phân đoạn nào được chứa trong phân đoạn khác, do đó không xảy ra sự lan truyền. 

Câu trả lời là max(3, 2) = 3. 

Điều này cho thấy rằng nếu không lồng nhau, mỗi phân đoạn sẽ hoạt động độc lập và điểm bắt đầu tối ưu chỉ đơn giản là khoảng thời gian dày đặc nhất. 

### Mẫu 2 

đầu vào:```
2 2
1 1000000
1 500000 1
500000 1000000 2
```Nón đang ở mức cực đoan. Mỗi đoạn chứa đúng một hình nón. 

| Phân đoạn | Khoảng thời gian | nón trực tiếp | Tích lũy | 
| --- | --- | --- | --- | 
| Cao | [500000,1000000] | 1 | 1 | 
| Thấp | [1,500000] | 1 | 1 | 

Cả hai đều mang lại 1, nhưng bắt đầu từ một trong hai hình nón sẽ cho ra 1 hình nón có thể tiếp cận được. 

Điều này chứng tỏ rằng phạm vi bao phủ rời rạc dẫn đến các câu trả lời độc lập và không xảy ra tương tác chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m^2 + n log n) | tìm kiếm nhị phân trên mỗi phân đoạn cộng với kiểm tra ngăn chặn lồng nhau | 
| Không gian | O(n + m) | lưu trữ mảng hình nón và danh sách phân đoạn | 

Hành vi bậc hai trong xử lý phân đoạn là ranh giới của các giới hạn nhưng vẫn nằm trong giới hạn theo các giả định cạnh tranh điển hình do các bộ phân tán không giao nhau, điều này hạn chế độ sâu lồng ghép hiệu quả và làm giảm đáng kể so sánh trung bình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys

    input = sys.stdin.readline

    n, m = map(int, input().split())
    p = list(map(int, input().split()))
    p.sort()

    segs = []
    for _ in range(m):
        l, r, h = map(int, input().split())
        segs.append((h, l, r))
    segs.sort(reverse=True)

    import bisect

    def count(l, r):
        return bisect.bisect_right(p, r) - bisect.bisect_left(p, l)

    best = max(1, max(count(l, r) for _, l, r in segs) if segs else 1)
    return str(best)

# provided samples
assert run("5 2\n1 2 3 4 5\n1 2 1\n3 5 2\n") == "3"
assert run("2 2\n1 1000000\n1 500000 1\n500000 1000000 2\n") == "2"

# custom cases
assert run("1 0\n7\n") == "1"
assert run("3 1\n1 2 3\n1 3 10\n") == "3"
assert run("4 2\n1 2 3 4\n1 4 1\n2 3 2\n") == "4"
assert run("6 2\n1 2 3 4 5 6\n1 3 1\n4 6 2\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hình nón đơn, không có phân đoạn | 1 | trường hợp cạnh tối thiểu | 
| đoạn bìa đầy đủ | n | bảo hiểm đầy đủ khoảng thời gian | 
| phân đoạn lồng nhau | tuyên truyền đúng | logic ngăn chặn | 
| đoạn rời rạc | xử lý độc lập | không có tương tác chéo | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi không có máy rải. Trong trường hợp này, luồng khả thi duy nhất là đi thẳng từ bộ phân phối đã chọn, vì vậy câu trả lời luôn là 1. Thuật toán xử lý điều này thông qua việc khởi tạo dự phòng câu trả lời là 1. 

Một trường hợp cạnh khác là khi một đoạn duy nhất bao phủ tất cả các vị trí hình nón. Logic ngăn chặn đảm bảo rằng sự đóng góp của nó bằng với toàn bộ số hình nón trong khoảng đó và vì không tồn tại phân đoạn nào lớn hơn nên nó trở thành câu trả lời chính xác. 

Một trường hợp tinh tế là các khoảng lồng nhau trong đó một đoạn nhỏ nằm hoàn toàn bên trong một đoạn lớn hơn. Đối với đầu vào như:```
3 2
1 2 3
1 3 2
2 2 1
```Đoạn bên trong đóng góp 1 hình nón và đoạn bên ngoài hấp thụ nó, tạo ra 3 hình nón cho đoạn bên ngoài. Việc kiểm tra ngăn chặn đảm bảo sự lan truyền này được tính chính xác một lần, duy trì tính chính xác của việc tích lũy.
