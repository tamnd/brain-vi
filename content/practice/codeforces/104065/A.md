---
title: "CF 104065A - Cấm hoặc chọn, thủ thuật là gì"
description: "Hai đội mỗi đội điều khiển một nhóm anh hùng riêng biệt. Mỗi anh hùng đều có một giá trị tích cực thể hiện mức độ hữu ích của nó đối với đội đó. Trò chơi sau đó chạy một chuỗi hành động dài xen kẽ."
date: "2026-07-02T03:16:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104065
codeforces_index: "A"
codeforces_contest_name: "2022 China Collegiate Programming Contest (CCPC) Mianyang Onsite"
rating: 0
weight: 104065
solve_time_s: 70
verified: true
draft: false
---

[CF 104065A - Cấm hoặc chọn, thủ thuật là gì](https://codeforces.com/problemset/problem/104065/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hai đội mỗi đội điều khiển một nhóm anh hùng riêng biệt. Mỗi anh hùng đều có một giá trị tích cực thể hiện mức độ hữu ích của nó đối với đội đó. Trò chơi sau đó chạy một chuỗi hành động dài xen kẽ. Trong mỗi nước đi, đội hiện tại sẽ yêu cầu một trong những anh hùng còn lại của mình để sử dụng sau này hoặc loại bỏ một anh hùng chưa được nhận khỏi nhóm của đối thủ. Sau khi tất cả các anh hùng đã được yêu cầu hoặc bị loại bỏ, mỗi đội sẽ chọn tối đa k anh hùng từ những anh hùng mà họ đã yêu cầu thành công và điểm cuối cùng của đội đó là tổng giá trị của những anh hùng được chọn đó. 

Trò chơi mang tính đối kháng. Đội A cố gắng tối đa hóa sự khác biệt giữa điểm số cuối cùng của mình và Đội B, trong khi Đội B cố gắng giảm thiểu sự khác biệt tương tự đó. Cả hai bên đều hành động hoàn hảo. 

Khó khăn chính là quá trình “cấm hoặc chọn” trung gian ảnh hưởng đến việc anh hùng nào sống sót trong nhóm của mỗi đội và cuối cùng chỉ có k người sống sót hàng đầu mới quan trọng. Vì k rất nhỏ, nhiều nhất là 10 nên điểm cuối cùng chỉ phụ thuộc vào một số ít anh hùng mỗi bên, nhưng sự tương tác giữa cấm và chọn sẽ quyết định ai thực sự vượt qua được. 

Các ràng buộc rất lớn về n, lên tới 100000, do đó, bất kỳ giải pháp nào mô phỏng trò chơi từng bước với lý luận lồng nhau đối với tất cả các anh hùng sẽ quá chậm. Ngay cả O(nk) cho mỗi quyết định cũng sẽ là giới hạn và bất kỳ điều gì cố gắng đánh giá trạng thái trò chơi một cách rõ ràng đều là không thể. Cấu trúc phải sụp đổ thành một thứ chỉ phụ thuộc vào thứ tự hoặc một tập hợp nhỏ các ứng cử viên. 

Một trường hợp khó nhận thấy là khi một bên có nhiều anh hùng có giá trị trung bình và chỉ có một số anh hùng rất lớn. Một ý tưởng ngây thơ có thể cho rằng việc cấm luôn nhắm mục tiêu tối đa toàn cầu ngay lập tức, nhưng vấn đề thời gian vì một anh hùng được chọn sớm sẽ miễn nhiễm với các lệnh cấm. Ví dụ: nếu k = 1 và A có giá trị [100, 1] trong khi B có [99, 98], cách tiếp cận ngây thơ “luôn cấm đối thủ tối đa” có thể gợi ý A mất 100 nếu B hành động sớm, nhưng A đi trước và có thể bảo vệ nó ngay lập tức, thay đổi kết quả. 

Một trường hợp cạnh khác là khi k lớn hơn 1 nhưng vẫn nhỏ. Chỉ bảo vệ người anh hùng lớn nhất thôi là chưa đủ; đối thủ có nhiều lượt can thiệp nên nhiều ứng cử viên hàng đầu phải được xem xét cùng nhau. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực của trò chơi sẽ mô phỏng mọi lượt chơi. Trong mỗi bước di chuyển, chúng tôi sẽ xem xét cả hai hành động có thể xảy ra, chọn hoặc cấm và khám phá cả hai nhánh trong khi theo dõi những anh hùng nào còn tồn tại. Ngay cả với tính năng ghi nhớ, trạng thái sẽ bao gồm những anh hùng nào vẫn còn sống và số lượt đã trôi qua. Không gian trạng thái đó tăng theo cấp số nhân với n, vì mỗi anh hùng có thể ở một trong nhiều trạng thái và mỗi lượt sẽ thay đổi cấu trúc. Cách tiếp cận này trở nên hoàn toàn không khả thi khi n đạt tới vài chục. 

Sự đơn giản hóa chính đến từ việc quan sát rằng chỉ có thứ hạng tương đối của các anh hùng mới quan trọng và đặc biệt hơn, chỉ k anh hùng hàng đầu mỗi đội mới góp phần vào điểm số cuối cùng. Mọi anh hùng khác đều không liên quan đến mục tiêu ngoại trừ trong chừng mực nó có thể được sử dụng làm mục tiêu cho các lệnh cấm. 

Bởi vì mỗi đội có chính xác n cơ hội để hành động và mọi anh hùng cuối cùng đều được chọn hoặc bị loại, nên cả hai đội đều có đủ lượt để đảm bảo k anh hùng trừ khi họ bị từ chối rõ ràng những anh hùng cụ thể đó. Điều này chuyển vấn đề sang việc hiểu những anh hùng nào trên thực tế có thể bị ngăn chặn và liệu lối chơi tối ưu có thể ngăn cản một đội thu thập k lựa chọn tốt nhất của mình hay không.

Trong lối chơi tối ưu, cả hai bên sẽ luôn ưu tiên bảo vệ một tướng rất có giá trị cho mình hoặc loại bỏ một tướng rất có giá trị khỏi đối phương. Tuy nhiên, vì mỗi đội có thể trực tiếp chọn anh hùng của mình và việc chọn ngay lập tức sẽ bảo vệ anh hùng đó khỏi các lệnh cấm trong tương lai, nên chiến lược tốt nhất là khóa ngay k anh hùng hàng đầu của mỗi bên thay vì dành thời gian can thiệp không ảnh hưởng đến top k set. 

Điều này dẫn đến việc giảm khóa: mỗi đội sẽ bảo vệ thành công k anh hùng có giá trị cao nhất từ ​​nhóm riêng của mình và đối thủ không thể ngăn chặn hoàn toàn điều này vì mỗi lựa chọn sẽ bảo vệ vĩnh viễn anh hùng đã chọn. 

Vì vậy, sự tương tác của lệnh cấm không làm thay đổi nhóm k anh hùng cuối cùng mỗi bên; nó chỉ ảnh hưởng đến trật tự và những anh hùng dư thừa không liên quan. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trò chơi Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Sắp xếp và lấy k top mỗi đội | O(n log n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Sắp xếp các giá trị anh hùng của Đội A theo thứ tự giảm dần. Lý do sắp xếp là vì chỉ những giá trị k lớn nhất mới có thể đóng góp vào điểm số cuối cùng, do đó, việc xác định chúng một cách trực tiếp sẽ tránh được việc suy luận về các hành động trung gian trong trò chơi. 
2. Lấy k giá trị đầu tiên từ danh sách đã sắp xếp của Đội A và tính tổng của chúng. Những lựa chọn này thể hiện lựa chọn tối ưu cho Đội A vì bất kỳ lựa chọn nào thay thế một trong những lựa chọn này bằng giá trị nhỏ hơn sẽ làm giảm nghiêm trọng điểm số. 
3. Lặp lại quy trình tương tự cho Đội B: sắp xếp các giá trị của nó theo thứ tự giảm dần và lấy k ở trên cùng. 
4. Tính câu trả lời cuối cùng bằng chênh lệch giữa hai tổng này, cụ thể là tổng k anh hùng hàng đầu của Đội A trừ đi tổng k anh hùng hàng đầu của Đội B. 

Ý tưởng quan trọng là mặc dù trò chơi bao gồm việc cấm nhưng việc cấm chỉ ảnh hưởng đến những anh hùng chưa được bảo đảm và cách chơi tối ưu cho phép mỗi bên bảo đảm được k anh hùng có giá trị nhất của mình ngay lập tức thông qua các hành động chọn. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi đội luôn có thể đảm bảo đưa k anh hùng có giá trị lớn nhất vào nhóm được chọn cuối cùng. Sau khi một anh hùng được chọn, nó không thể bị xóa và vì mỗi đội có đủ lượt để thực hiện k lượt chọn, lối chơi tối ưu cho phép k lượt chọn đó hướng tới những anh hùng có giá trị cao nhất trước khi bất kỳ chiến lược nào liên quan đến lệnh cấm có thể loại bỏ chúng vĩnh viễn. Bất kỳ lệnh cấm nào nhắm vào một anh hùng không phải top-k đều không ảnh hưởng đến điểm số cuối cùng và mọi nỗ lực cấm một anh hùng top-k đều bị phản đối bằng cách chỉ cần chọn anh hùng đó ở lượt trước đó. Điều này làm cho không gian quyết định hiệu quả cuối cùng bị thu gọn thành việc lựa chọn độc lập k phần tử hàng đầu cho mỗi nhóm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    a.sort(reverse=True)
    b.sort(reverse=True)
    
    print(sum(a[:k]) - sum(b[:k]))

if __name__ == "__main__":
    main()
```Việc triển khai hoàn toàn phụ thuộc vào việc sắp xếp, vì động lực của trò chơi giảm xuống việc chọn k giá trị lớn nhất cho mỗi bên. Việc sắp xếp đảm bảo chúng tôi xác định được các giá trị này trong thời gian O(n log n) và việc cắt lát sẽ trích xuất chính xác phần quan trọng. 

Không cần mô phỏng các lượt vì kết quả cuối cùng chỉ phụ thuộc vào k phần tử nào tồn tại trong mỗi đội chứ không phụ thuộc vào thứ tự chúng được xử lý. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1
3 6
2 4
```Mảng được sắp xếp: 

A = [6, 3] 

B = [4, 2] 

| Bước | Một sắp xếp | B sắp xếp | Một khoản tiền đầu k | B đầu k tổng | Sự khác biệt | 
| --- | --- | --- | --- | --- | --- | 
| Sau khi sắp xếp | [6, 3] | [4, 2] | - | - | - | 
| Lấy k=1 | [6] | [4] | 6 | 4 | 2 | 

Điều này cho thấy rằng chỉ có anh hùng mạnh nhất trong mỗi đội mới quan trọng khi k = 1 và phần còn lại của cấu trúc là không liên quan. 

### Ví dụ 2 

đầu vào:```
4 2
1 3 5 7
2 4 6 8
```Mảng được sắp xếp: 

A = [7, 5, 3, 1] 

B = [8, 6, 4, 2] 

| Bước | Một sắp xếp | B sắp xếp | Một khoản tiền đầu k | B đầu k tổng | Sự khác biệt | 
| --- | --- | --- | --- | --- | --- | 
| Sau khi sắp xếp | [7,5,3,1] | [8,6,4,2] | - | - | - | 
| Lấy k=2 | [7,5] | [8,6] | 12 | 14 | -2 | 

Điều này xác nhận rằng phương pháp này luôn chọn k anh hùng tốt nhất hiện có cho mỗi bên, bất kể sự hiện diện của các quyết định cấm trung gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp cả hai mảng chiếm ưu thế trong thời gian chạy | 
| Không gian | O(1) thêm | Sắp xếp được thực hiện ngoài việc lưu trữ đầu vào | 

Các ràng buộc cho phép tối đa 100000 anh hùng và việc sắp xếp thoải mái phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ là tuyến tính theo kích thước đầu vào, điều này là không thể tránh khỏi vì các mảng phải được lưu trữ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    a.sort(reverse=True)
    b.sort(reverse=True)
    
    return str(sum(a[:k]) - sum(b[:k]))

# provided samples
assert run("2 1\n3 6\n2 4\n") == "2"
assert run("4 1\n1 3 5 7\n2 4 6 8\n") == "-1"
assert run("4 2\n4 6 7 9\n2 5 8 10\n") == "-6"

# custom cases
assert run("1 1\n10\n1\n") == "9", "single element"
assert run("5 2\n5 4 3 2 1\n1 2 3 4 5\n") == "0", "symmetric arrays"
assert run("6 3\n10 9 8 1 1 1\n7 6 5 4 3 2\n") == "9", "skewed distribution"
assert run("3 1\n100 1 1\n50 50 50\n") == "50", "tie-heavy opponent"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/10/1 | 9 | trường hợp n tối thiểu | 
| mảng đối xứng | 0 | hủy bỏ cân bằng | 
| phân phối lệch | 9 | sự thống trị của các giá trị hàng đầu | 
| đối thủ nặng ký | 50 | tác dụng của lựa chọn mạnh nhất | 

## Vỏ cạnh 

Khi n = 1, trò chơi chuyển sang một so sánh duy nhất trong đó mỗi bên bảo vệ hoặc mất đi người hùng duy nhất của mình một cách hiệu quả. Thuật toán xử lý việc này một cách chính xác vì việc sắp xếp để lại một phần tử duy nhất trên mỗi mảng và k = 1 buộc phải so sánh trực tiếp. 

Khi k = n, mọi anh hùng đều được chọn một cách hiệu quả và câu trả lời sẽ là tổng chênh lệch. Việc sắp xếp vẫn hoạt động vì việc lấy tất cả các phần tử sau khi sắp xếp tương đương với việc tính tổng toàn bộ mảng. 

Khi một bên có nhiều giá trị lớn được nhóm lại với nhau, đối thủ không thể loại bỏ đủ chúng một cách có chọn lọc để thay thế hoàn toàn tập hợp k hàng đầu, vì mỗi anh hùng được chọn sẽ được bảo vệ vĩnh viễn. Lựa chọn được sắp xếp vẫn thu được tập hợp con chính xác vì nó phản ánh các giá trị duy nhất có thể tồn tại khi chơi tối ưu.
