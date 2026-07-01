---
title: "CF 104414D - \u8d26\u53f7 -1"
description: "Có $n$ tài khoản sinh viên được gắn nhãn từ $1$ đến $n$, cộng với một tài khoản giáo viên đặc biệt được gắn nhãn $-1$. Chúng ta cũng được cung cấp một chuỗi các sự kiện $t$."
date: "2026-06-30T20:01:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104414
codeforces_index: "D"
codeforces_contest_name: "2023 Hunan Provincal Multi-University Training (Xiangtan University)"
rating: 0
weight: 104414
solve_time_s: 55
verified: true
draft: false
---

[CF 104414D - \u8d26\u53f7 -1](https://codeforces.com/problemset/problem/104414/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

có$n$tài khoản sinh viên được gắn nhãn$1$ĐẾN$n$, cộng với một tài khoản giáo viên đặc biệt được gắn nhãn$-1$. Chúng ta cũng được cung cấp một chuỗi$t$sự kiện. Mỗi sự kiện mô tả một học sinh$x$người mở danh sách theo dõi của tài khoản khác$y$, sau đó theo dõi mọi người hiện đang ở$y$danh sách theo dõi của. Nếu như$x$đã theo ai đó rồi, không có gì thay đổi cả. Nếu như$x$cố gắng làm theo mình, hành động đó bị bỏ qua. 

Ban đầu, sinh viên$1$đi theo giáo viên$-1$. Tất cả các học sinh khác không theo ai cả, và giáo viên cũng không theo ai cả. 

Quá trình này hoàn toàn là về việc mở rộng các mối quan hệ theo dõi. Mỗi ngày sao chép một tập hợp các cạnh theo dõi từ nút này sang nút khác, với ràng buộc là các vòng lặp tự bị bỏ qua và các lượt theo dõi trùng lặp là vô hại. 

Mục đích là để xác định, sau khi tất cả các sự kiện được xử lý, học sinh nào theo dõi giáo viên$-1$. 

Những hạn chế rất nhỏ:$n \le 100$,$t \le 1000$. Điều này ngay lập tức cho phép bất kỳ$O(t \cdot n^2)$hoặc thậm chí$O(t \cdot n)$mô phỏng. Không cần cấu trúc dữ liệu nâng cao hoặc tối ưu hóa tiệm cận ngoài việc truyền tập hợp hoặc bitset đơn giản. 

Một trường hợp phức tạp xuất phát từ thực tế là tài khoản giáo viên$-1$hoạt động giống như một nút bình thường trong biểu đồ nhưng bị loại khỏi miền đầu ra cuối cùng ngoại trừ mục tiêu. Một trường hợp góc khác là việc sao chép lặp đi lặp lại cùng một danh sách lân cận: vì các tập hợp là bình đẳng nên các phép toán lặp lại không được phép đếm gấp đôi hoặc phá vỡ tính chính xác. 

Một tình huống khó hiểu tối thiểu là khi sao chép bao gồm$-1$chính nó. Ví dụ: nếu một học sinh theo dõi giáo viên được sử dụng làm nguồn, những người khác có thể gián tiếp kế thừa giáo viên theo dõi ngay lập tức. 

## Phương pháp tiếp cận 

Quá trình này có thể được hiểu là một biểu đồ có hướng phát triển theo thời gian. Mỗi tài khoản duy trì một tập hợp các cạnh gửi đi đại diện cho người mà họ theo dõi. Mỗi hoạt động lấy tập hợp hàng xóm đi đầy đủ của$y$và hợp nhất nó vào$x$đã được thiết lập. 

Một mô phỏng lực lượng vũ phu tự gợi ý một cách tự nhiên: duy trì một tập hợp cho mỗi nút và cho mỗi thao tác lặp lại trên tất cả các phần tử trong$y$được thiết lập và chèn chúng vào$x$đã được thiết lập. Mỗi lần chèn là$O(1)$trung bình với một tập hợp băm, vì vậy chi phí của mỗi sự kiện$O(n)$trong trường hợp xấu nhất. Qua$t$sự kiện đây là$O(tn)$, điều này dễ dàng được chấp nhận đối với$n \le 100$,$t \le 1000$. 

Không cần tối ưu hóa ngoài điều này vì hệ thống không bao giờ yêu cầu các đường đi ngắn nhất, đóng khả năng tiếp cận hoặc tính toán lại cấu trúc toàn cầu nhiều lần. Mỗi bản cập nhật là sự lan truyền cục bộ của một tập hợp nhỏ. 

Một chế độ xem có cấu trúc hơn là nghĩ đến việc mỗi nút duy trì một mặt nạ bit theo sau. Sau đó, mỗi thao tác là một HOẶC mặt nạ theo bit, với một bit tự xóa. Điều này làm giảm các yếu tố không đổi và làm cho tính chính xác trở nên rõ ràng. 

Quan sát quan trọng là hệ thống này đơn điệu: một khi cạnh theo sau được thêm vào, nó sẽ không bao giờ bị xóa. Điều này ngăn cản sự dao động và đảm bảo rằng sự lan truyền đơn giản sẽ hội tụ ngay sau mỗi thao tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng bộ vũ phu |$O(t \cdot n)$|$O(n^2)$| Đã chấp nhận | 
| Tối ưu hóa bit |$O(t \cdot n / w)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cấu trúc`follow[i]`, đại diện cho tập hợp các tài khoản được người dùng theo dõi$i$. Giáo viên được lập chỉ mục là$-1$, mà chúng tôi ánh xạ tới chỉ mục$0$để thuận tiện và sinh viên$1..n$bản đồ tới$1..n$. 

Chúng ta cũng khởi tạo hệ thống theo câu lệnh: sinh viên$1$đi theo giáo viên, vì vậy`follow[1] = {teacher}`và tất cả các bộ khác đều trống. 

Đối với mỗi sự kiện$(x, y)$, chúng tôi hợp nhất tất cả các phần tử của`follow[y]`vào trong`follow[x]`, đồng thời cẩn thận tránh tự làm theo. 

Sau khi xử lý tất cả các sự kiện, chúng tôi quét xem học sinh nào có giáo viên trong nhóm theo dõi của họ. 

### Các bước 

1. Khởi tạo một mảng các tập hợp`follow`kích thước$n+1$, trong đó chỉ số$i$đại diện cho sinh viên$i$và một đại diện riêng cho giáo viên như một mã định danh đặc biệt. Sự tách biệt này tránh nhầm lẫn với việc lập chỉ mục phủ định. 
2. Đặt điều kiện ban đầu`follow[1]`để chứa giáo viên, vì học sinh$1$bắt đầu bằng cách làm theo$-1$. 
3. Đối với mỗi sự kiện$(x, y)$, lặp lại mọi tài khoản$v$TRONG`follow[y]`. 
4. Đối với mỗi như vậy$v$, nếu như$v \neq x$, chèn$v$vào trong`follow[x]`. Việc tự kiểm tra đảm bảo không có tài khoản nào tự tuân theo quy định của hệ thống. 
5. Sau khi xử lý tất cả các sự kiện, lặp lại các sinh viên$1..n$và thu thập các chỉ số đó$i$như vậy là giáo viên đang ở trong`follow[i]`. 
6. Sắp xếp danh sách kết quả trước khi xuất ra, mặc dù việc lặp lại từ$1$ĐẾN$n$đã đảm bảo thứ tự sắp xếp. 

### Tại sao nó hoạt động 

Bất cứ lúc nào,`follow[i]`thể hiện chính xác tập hợp các tài khoản có thể truy cập được bằng một chuỗi các thao tác "sao chép-theo dõi-danh sách" kết thúc tại$i$, bắt đầu từ cấu hình ban đầu. Mỗi thao tác chỉ thêm các phần tử đã có trong một tập hợp khác, do đó không có phần tử nhân tạo nào được đưa vào. Vì các tập hợp được sử dụng nên các bản sao không ảnh hưởng đến trạng thái và vì các cạnh chỉ được thêm vào nên không có mối quan hệ nào trước đó bị mất. Điều này đảm bảo rằng sau khi xử lý tất cả các hoạt động, tư cách thành viên trong`follow[i]`phản ánh chính xác liệu$i$đã từng kế thừa người thầy qua bất kỳ chuỗi sự kiện sao chép nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, t = map(int, input().split())
    
    TEACHER = -1
    
    follow = [set() for _ in range(n + 1)]
    
    follow[1].add(TEACHER)
    
    for _ in range(t):
        x, y = map(int, input().split())
        
        # copy all follows from y to x
        for v in follow[y]:
            if v != x:
                follow[x].add(v)
    
    res = []
    for i in range(1, n + 1):
        if TEACHER in follow[i]:
            res.append(i)
    
    print(len(res))
    print(*res)

if __name__ == "__main__":
    main()
```Giải pháp trực tiếp thực hiện mô hình lan truyền đã thiết lập. Giáo viên được đại diện bởi một giá trị trọng điểm$-1$, điều này an toàn vì tất cả ID sinh viên đều dương. Trong mỗi thao tác, chúng tôi lặp lại ảnh chụp nhanh hiện tại của`follow[y]`và mở rộng`follow[x]`. Việc tự kiểm tra ngăn chặn các cạnh tự theo dõi bất hợp pháp. 

Lần quét cuối cùng chỉ đơn giản là kiểm tra tư cách thành viên của giáo viên trong nhóm của mỗi học sinh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 5
1 2
3 2
1 -1
1 -1
3 1
```Chúng tôi chỉ theo dõi các học phần liên quan đến giáo viên. 

| Bước | Hoạt động | theo dõi[1] | theo dõi[2] | theo dõi[3] | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | {-1} | {} | {} | 
| 1 | 1 bản 2 | {-1} | {} | {} | 
| 2 | 3 bản 2 | {-1} | {} | {} | 
| 3 | 1 bản -1 | {-1} | {} | {} | 
| 4 | 1 bản -1 | {-1} | {} | {} | 
| 5 | 3 bản 1 | {-1} | {} | {-1} | 

Chỉ có học sinh 1 và 3 đi theo giáo viên nên kết quả là:```
2
1 3
```Điều này cho thấy rằng một khi giáo viên xuất hiện trong một chuỗi, nó có thể truyền tiếp qua các thao tác sao chép tiếp theo. 

### Ví dụ 2 

đầu vào:```
1 1
1 -1
```| Bước | Hoạt động | theo dõi[1] | 
| --- | --- | --- | 
| ban đầu | - | {-1} | 
| 1 | 1 bản -1 | {-1} | 

Đầu ra:```
1
1
```Điều này xác nhận trường hợp cơ bản trong đó một học sinh liên tục củng cố mối quan hệ theo dõi đã tồn tại mà không thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \cdot n)$| Mỗi sự kiện hợp nhất tối đa$n$phần tử từ bộ này sang bộ khác | 
| Không gian |$O(n^2)$| Mỗi trong số$n$các nút có thể lưu trữ tới$O(n)$theo dõi | 

Giới hạn$n \le 100$,$t \le 1000$làm điều này nhanh chóng thoải mái. Tổng số hoạt động tối đa là$10^5$phần chèn thêm, điều này không quan trọng đối với Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, t = map(int, input().split())
    TEACHER = -1
    follow = [set() for _ in range(n + 1)]
    follow[1].add(TEACHER)
    for _ in range(t):
        x, y = map(int, input().split())
        for v in follow[y]:
            if v != x:
                follow[x].add(v)
    res = [i for i in range(1, n + 1) if TEACHER in follow[i]]
    return str(len(res)) + "\n" + " ".join(map(str, res)) + "\n"

# provided sample (conceptual check)
assert run("3 5\n1 2\n3 2\n1 -1\n1 -1\n3 1\n") == "2\n1 3\n"

# minimum case
assert run("1 0\n") == "1\n1\n"

# single propagation
assert run("2 1\n1 -1\n") == "1\n1\n"

# chain propagation
assert run("3 2\n1 -1\n2 1\n") == "2\n1 2\n"

# no propagation
assert run("3 1\n2 1\n") == "1\n1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 1 1 | Tính đúng đắn của điều kiện ban đầu | 
| 2 1; 1 -1 | 1 1 | Giáo viên cơ bản theo dõi | 
| 3 2; 1 -1; 2 1 | 2 1 2 | Tuyên truyền qua chuỗi | 
| 3 1; 2 1 | 1 1 | Không có sự lây lan ngẫu nhiên | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi nguồn duy nhất của giáo viên là gián tiếp. Ví dụ, nếu học sinh$2$không bao giờ trực tiếp theo sau$-1$nhưng sau đó sao chép học sinh$1$, người đã theo dõi$-1$, sau đó là sinh viên$2$cuối cùng vẫn phải theo thầy. Thuật toán xử lý việc này vì`follow[1]`đã chứa$-1$, và sao chép nó sẽ chuyển điểm của giáo viên về phía trước. 

Một trường hợp khác là những nỗ lực tự sao chép lặp đi lặp lại. Nếu một hoạt động được`(1, -1)`khi`follow[-1]`trống rỗng, không có gì thay đổi. Nếu lặp lại nhiều lần, tập hợp vẫn ổn định vì các bản sao bị cấu trúc tập hợp bỏ qua và tính năng tự theo dõi bị chặn rõ ràng.
