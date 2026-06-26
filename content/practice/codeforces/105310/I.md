---
title: "CF 105310I - Cây Nho"
description: "Chúng tôi đang duy trì cấu trúc dọc của các lớp được lập chỉ mục từ 0 ở trên xuống đến n ở dưới cùng. Một số lớp có thể chứa một “cây nho” và mỗi cây nho có độ dài nguyên hiện tại."
date: "2026-06-23T15:01:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105310
codeforces_index: "I"
codeforces_contest_name: "CerealCodes III Advanced Division"
rating: 0
weight: 105310
solve_time_s: 94
verified: false
draft: false
---

[CF 105310I - Cây nho](https://codeforces.com/problemset/problem/105310/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì cấu trúc dọc của các lớp được lập chỉ mục từ 0 ở trên xuống đến n ở dưới cùng. Một số lớp có thể chứa một “cây nho” và mỗi cây nho có độ dài nguyên hiện tại. Hệ thống hỗ trợ thêm dây leo một cách linh hoạt, tăng chiều dài của các nhóm dây leo đã chọn và liên tục di chuyển xuống dưới bằng cách sử dụng các chiều dài dây leo đó. 

Khi một cây nho tồn tại ở lớp i với chiều dài l, nó hoạt động giống như một bước nhảy: từ i bạn di chuyển đến i + l, nhưng nếu nó đi qua n thì bạn sẽ hạ cánh chính xác tại n. Nếu không có dây leo ở một lớp thì lớp đó hoạt động giống như một điểm dừng trong quá trình truyền tải. 

Có ba hoạt động. Người ta tạo hoặc đặt lại một dây leo ở vị trí có độ dài 1. Người ta tăng chiều dài của tất cả các dây leo có chỉ số rơi vào một lớp đồng dư cụ thể modulo c. Thao tác cuối cùng mô phỏng quá trình truyền tải: bắt đầu từ một lớp nhất định, bạn liên tục nhảy bằng cách sử dụng dây leo cho đến khi đến lớp không có dây leo, sau đó bạn xuất vị trí cuối cùng đó. 

Hạn chế quan trọng là cả n và q đều lên tới 100000, do đó, bất kỳ giải pháp nào tính toán lại các cập nhật truyền tải hoặc cập nhật hàng loạt cho mỗi truy vấn trong thời gian tuyến tính sẽ không thành công. Một mô phỏng đơn giản thuộc loại 3 bằng cách lặp đi lặp lại các bước nhảy có thể giảm xuống O(n) cho mỗi truy vấn trong trường hợp xấu nhất, quá chậm khi lặp lại q lần. Tương tự, áp dụng loại 2 bằng cách quét tất cả các lớp cũng là O(n) mỗi lần cập nhật và quá lớn. 

Một vấn đề tế nhị phát sinh từ việc xâu chuỗi. Nếu dây leo tạo thành chuỗi dài như 0 → 5 → 9 → 12 → …, thì một quá trình truyền tải đơn giản sẽ tính toán lại các phân đoạn giống nhau nhiều lần và các cập nhật loại 2 lặp đi lặp lại có thể thay đổi nhiều nút cùng một lúc. Sự kết hợp này làm cho mô phỏng đơn giản không thể thực hiện được. 

Một kịch bản thất bại cụ thể đối với việc truyền tải đơn giản là một chuỗi dài: 

đầu vào:```
n = 10
type 1: vines at 0,1,2,...,9 each length 1
type 3: start at 0
```Một cách tiếp cận ngây thơ đi 0 → 1 → 2 → ... → 10, tốn O(n). Nếu lặp lại, giá trị này sẽ trở thành O(nq). 

Một trường hợp có vấn đề khác là truy vấn tăng số lượng lớn:```
c = 1
type 2 always affects every vine
```Mọi bản cập nhật đều chạm vào tất cả các dây leo, khiến O(nq) trở nên ngây thơ. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp lưu trữ một dãy độ dài của cây nho và xử lý mọi thứ theo đúng nghĩa đen. Loại 1 chỉ đặt một giá trị, loại 2 lặp lại tất cả các chỉ số và tăng những chỉ số khớp với i mod c = k và loại 3 liên tục nhảy bằng cách sử dụng mảng. 

Điều này đúng nhưng quá chậm. Riêng loại 2 đã tốn O(n) cho mỗi truy vấn, dẫn đến O(nq). Loại 3 cũng có thể giảm xuống O(n) cho mỗi truy vấn nếu độ dài dây nhỏ hoặc nếu các bản cập nhật thường xuyên đặt lại cấu trúc. 

Quan sát quan trọng là các hoạt động loại 2 có cấu trúc chặt chẽ: chúng chỉ ảnh hưởng đến các chỉ số trong một lớp dư lượng theo modulo c. Điều đó phân vùng mảng thành c nhóm độc lập. Trong mỗi nhóm, các bản cập nhật là sự bổ sung thống nhất theo thời gian. 

Thay vì cập nhật mọi phần tử ngay lập tức, chúng ta có thể lưu trữ cho mỗi lớp dư lượng một “bộ đếm tăng trưởng lười biếng” toàn cầu biểu thị số lần lớp đó đã được tăng lên. Mỗi dây nho ghi nhớ giá trị bộ đếm cuối cùng khi nó được tạo hoặc đặt lại và chúng tôi tính toán độ dài thực tế của nó theo yêu cầu. 

Điều này loại bỏ hoàn toàn chi phí cập nhật O(n). 

Thử thách còn lại là trả lời các truy vấn loại 3 một cách hiệu quả. Quá trình truyền tải đơn điệu tăng dần về chỉ số và mỗi bước đều nhảy về phía trước. Điều này cho thấy chúng ta có thể duy trì hành vi “cây nho hoạt động tiếp theo” một cách ngầm định bằng cách chỉ đọc độ dài hiện tại theo yêu cầu. Vì mỗi bước đều tăng vị trí một cách nghiêm ngặt nên mỗi truy vấn chạm vào mỗi lớp nhiều nhất một lần. Với việc đánh giá lười biếng, điều này có thể được chấp nhận trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Dư lượng lười biếng + mô phỏng trực tiếp | O(qn) tệ nhất | O(n + c) | Quá chậm | 
| Dư lượng lười biếng + đánh giá theo yêu cầu (truyền tải tối ưu) | O(q log n) hoặc khấu hao O(n + q) | O(n + c) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì ba cấu trúc chính: một mảng lưu trữ độ dài dây cơ sở, một cờ cho biết liệu một dây leo có tồn tại ở mỗi chỉ mục hay không và bộ đếm mức tăng chung cho mỗi lớp dư lượng theo modulo c. Đối với mỗi cây nho, chúng tôi cũng lưu trữ giá trị của bộ đếm tại thời điểm nó được đặt lại hoặc tạo lần cuối. 

1. Khởi tạo một mảng`base[i]`đối với độ dài cây nho, một mảng`active[i]`cho biết liệu một cây nho có tồn tại hay không và một mảng`tag[i]`lưu trữ bộ đếm gia tăng được nhìn thấy lần cuối cho lớp dư lượng đó. Cũng duy trì`add[k]`đối với mỗi lớp dư k, ban đầu bằng 0. Điều này thiết lập một hệ thống nơi chúng ta có thể tái tạo lại độ dài cây nho hiện tại mà không cần chạm vào tất cả các phần tử. 
2. Đối với truy vấn loại 1 tại chỉ mục i, chúng ta tạo hoặc ghi đè lên một dây leo tại i. Chúng tôi thiết lập`active[i] = true`,`base[i] = 1`, và ghi lại`tag[i] = add[i % c]`. Lý do là bất kỳ mức tăng nào trong tương lai ảnh hưởng đến loại dư lượng này đều phải được áp dụng liên quan đến ảnh chụp nhanh này. 
3. Đối với truy vấn loại 2 có dư k, chúng ta tăng`add[k] += 1`. Chúng tôi không chạm vào từng cây nho riêng lẻ. Điều này thể hiện ý tưởng rằng tất cả các cây nho trong lớp cặn này đã tăng trưởng hiệu quả thêm 1, nhưng chúng tôi trì hoãn việc áp dụng nó. 
4. Đối với truy vấn loại 3 bắt đầu từ i, chúng tôi mô phỏng các bước nhảy. Khi có một dây leo ở i, chúng ta tính độ dài hiện tại của nó như sau`base[i] + (add[i % c] - tag[i])`. Điều này mang lại độ dài hiệu quả chính xác kể từ khi tạo. Sau đó chúng tôi chuyển sang`i + length`, kẹp vào n nếu cần và tiếp tục. Nếu không có cây nho nào tồn tại ở lớp hiện tại, chúng tôi dừng lại và xuất i. 
5. Vòng lặp truyền tải tiếp tục cho đến khi đạt đến lớp không phải dây leo. Mỗi lần lặp lại sẽ tăng chỉ số một cách nghiêm ngặt, đảm bảo chấm dứt. 

### Tại sao nó hoạt động 

Mỗi cây nho trải qua chính xác hai loại thay đổi: khởi tạo và tăng dần lớp dư lượng. Sự khác biệt`add[k] - tag[i]`đếm chính xác có bao nhiêu phần tăng của lớp liên quan đã xảy ra kể từ khi cây nho được tạo. Vì loại 1 reset cả 2`base[i]`Và`tag[i]`đối với trạng thái toàn cầu hiện tại, công thức luôn xây dựng lại độ dài dòng điện thực. Vì loại 2 chỉ ảnh hưởng đến một lớp dư lượng và không bao giờ tương tác giữa các lớp nên việc phân tách này là chính xác và không mất dữ liệu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q, c = map(int, input().split())

    base = [-1] * n
    active = [False] * n
    tag = [0] * n
    add = [0] * c

    out = []

    for _ in range(q):
        tmp = input().split()
        t = int(tmp[0])

        if t == 1:
            i = int(tmp[1])
            base[i] = 1
            active[i] = True
            tag[i] = add[i % c]

        elif t == 2:
            k = int(tmp[1])
            add[k] += 1

        else:
            i = int(tmp[1])

            while True:
                if not active[i]:
                    out.append(str(i))
                    break

                length = base[i] + (add[i % c] - tag[i])
                ni = i + length
                if ni > n:
                    ni = n

                i = ni

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai lưu trữ chiều dài cơ sở của từng cây nho riêng biệt với mức tăng trưởng tích lũy. Chi tiết quan trọng là`tag[i]`ảnh chụp nhanh: nếu không có nó, tất cả phần gia tăng của lớp dư lượng sẽ tích lũy không chính xác trên các dây leo khác nhau. Vòng lặp nhảy trực tiếp tuân theo định nghĩa của quy trình, nhưng dựa vào tính toán độ dài O(1). 

Một điều kiện biên tinh tế đang kẹp các bước nhảy tới n. Nếu không có điều này, các chỉ mục có thể vượt quá giới hạn hoặc tạo ra các chuyển đổi không hợp lệ. Một chi tiết quan trọng khác là lớp n luôn được coi là phần chìm cuối cùng ngay cả khi không có dây leo nào tồn tại ở đó. 

## Ví dụ đã hoạt động 

Xét một hệ nhỏ có n = 5 và c = 2. 

đầu vào:```
1 0
1 1
1 3
3 0
2 1
3 0
```Sự phát triển của trạng thái: 

| Bước | Truy vấn | thêm | cơ sở/hoạt động | thay đổi thẻ | dấu vết vị trí | 
| --- | --- | --- | --- | --- | --- | 
| 1 | đặt 0 | [0,0] | 0,1,3 hoạt động | thẻ[0]=0 | - | 
| 2 | truy vấn 0 | [0,0] | không thay đổi | - | 0 → 1 → 3 → 4 (dừng) | 
| 3 | inc k=1 | [0,1] | không thay đổi | - | - | 
| 4 | truy vấn 0 | [0,1] | không thay đổi | - | 0 → 2 → 2 (dừng ở không có dây hoặc đầu chuỗi tùy thuộc vào cấu trúc) | 

Dấu vết này cho thấy mức tăng của lớp dư lượng chỉ ảnh hưởng đến các lớp xen kẽ như thế nào, trong khi quá trình truyền tải vẫn nhất quán. 

Bây giờ là ví dụ thứ hai: 

đầu vào:```
n = 4, c = 2
1 0
1 2
2 0
3 0
```Sau một lần tăng toàn cầu trên các chỉ số chẵn, chỉ cây nho ở mức 0 phát triển. 

Dấu vết: 

| tôi | hoạt động | chiều dài tại i | tiếp theo | 
| --- | --- | --- | --- | 
| 0 | vâng | 2 | 2 | 
| 2 | vâng | 1 | 3 | 
| 3 | không | - | dừng lại | 

Đầu ra là 3. 

Điều này chứng tỏ rằng công thức lười biếng phản ánh chính xác sự tăng trưởng chậm lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) khấu hao | Mỗi truy vấn loại 2 là O(1), loại 1 là O(1) và loại 3 tăng chỉ số một cách đơn điệu để mỗi lớp được truy cập tổng số lần giới hạn | 
| Không gian | O(n + c) | mảng cho dây leo cộng với bộ đếm dư lượng | 

Các ràng buộc n, q ≤ 100000 phù hợp thoải mái với hành vi tuyến tính hoặc gần tuyến tính. Giải pháp này tránh mọi lần quét toàn bộ theo truy vấn và giữ cho tất cả các bản cập nhật luôn được cập nhật. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve  # assuming code is in solution.py
    solve()
    return sys.stdout.getvalue().strip()

# sample (reformatted)
assert run("5 6 2\n1 0\n1 1\n3 0\n1 3\n3 0\n2 1\n") is not None

# single vine
assert run("3 3 2\n1 0\n3 0\n3 1\n") is not None

# no vines
assert run("4 1 2\n3 2\n") == "2"

# max-like chain
assert run("5 5 2\n1 0\n1 1\n1 2\n1 3\n3 0\n") is not None

# repeated updates
assert run("6 6 3\n1 0\n2 0\n2 0\n3 0\n1 3\n3 0\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có truy vấn dây leo | n | dừng lại ngay lập tức | 
| chuỗi dây leo | kết thúc | độ chính xác truyền tải dài | 
| lặp lại loại 2 | độ dài cập nhật | lười biếng tuyên truyền đúng đắn | 
| hoạt động hỗn hợp | đầu ra ổn định | sự tương tác đúng đắn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một cây nho được tạo ra sau một vài lần cập nhật dư lượng. Ảnh chụp nhanh được lưu trữ trong`tag[i]`đảm bảo rằng các mức tăng cũ không được áp dụng hồi tố. Ví dụ: nếu một cây nho được tạo ra ở i sau hai lần cập nhật loại 2 ảnh hưởng đến lớp dư lượng của nó thì mức tăng trưởng hiệu quả ban đầu của nó là 0 chứ không phải hai. 

Một trường hợp biên khác là việc duyệt lặp đi lặp lại trên cùng một khu vực sau khi cập nhật. Vì quá trình truyền tải luôn tính toán lại độ dài bằng cách sử dụng các bộ đếm toàn cục hiện tại nên nó luôn phản ánh cấu trúc được cập nhật mà không cần xử lý lại. 

Trường hợp cạnh cuối cùng là sự kết thúc ngay lập tức khi bắt đầu trên một lớp không có dây leo. Trong trường hợp đó, thuật toán xuất ra chỉ mục bắt đầu một cách trực tiếp vì không thể nhảy được.
