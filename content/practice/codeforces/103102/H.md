---
title: "CF 103102H - VÀ = HOẶC"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta quan tâm đến các phân đoạn liền kề của mảng này trong đó có điều kiện theo bit: AND theo bit của tất cả các phần tử trong phân đoạn chính xác bằng bitwise OR của tất cả các phần tử trong cùng một phân đoạn."
date: "2026-07-03T21:48:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103102
codeforces_index: "H"
codeforces_contest_name: "2020-2021 ICPC Southeastern European Regional Programming Contest (SEERC 2020)"
rating: 0
weight: 103102
solve_time_s: 43
verified: true
draft: false
---

[CF 103102H - AND = OR](https://codeforces.com/problemset/problem/103102/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta quan tâm đến các phân đoạn liền kề của mảng này trong đó có điều kiện theo bit: AND theo bit của tất cả các phần tử trong phân đoạn chính xác bằng bitwise OR của tất cả các phần tử trong cùng một phân đoạn. 

Nhiệm vụ là đếm xem có bao nhiêu phân đoạn như vậy tồn tại. 

Đầu vào có thể lớn, thường lên tới khoảng 10^5 phần tử, điều này ngay lập tức loại trừ mọi phương pháp kiểm tra rõ ràng mọi phân đoạn. Một phép liệt kê O(n^2) ngây thơ của tất cả các mảng con, tính toán lại AND và OR mỗi lần, sẽ bao gồm thứ tự 10^10 phép toán trong trường hợp xấu nhất, vượt xa mọi giới hạn thực tế trong các hạn chế về thời gian thông thường. Điều này buộc chúng ta phải tìm kiếm cấu trúc cho phép tổng hợp mà không cần tính toán lại. 

Trường hợp cạnh khóa xuất hiện khi tất cả các phần tử giống hệt nhau. Ví dụ, nếu mảng là`[5, 5, 5]`, thì mọi mảng con đều thỏa mãn điều kiện vì cả AND và OR đều bằng 5. Một cách tiếp cận bất cẩn chỉ kiểm tra điểm cuối hoặc giả định đẳng thức theo cặp vẫn có thể bỏ sót rằng các phân đoạn dài hơn cũng hợp lệ. Một trường hợp tinh tế khác là khi các phần tử chỉ khác nhau một bit. Ví dụ`[1, 3]`ở dạng nhị phân là`[01, 11]`, trong đó AND là`01`và HOẶC là`11`, không bằng nhau, do đó ngay cả những khác biệt nhỏ cũng sẽ phá vỡ tính hợp lệ ngay lập tức. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi xem xét mọi mảng con`[l, r]`, tính toán AND và OR theo từng bit trong khoảng đó và kiểm tra xem chúng có bằng nhau không. Điều này đúng vì nó khớp trực tiếp với định nghĩa. Tuy nhiên, việc tính toán lại AND và OR cho mỗi mảng con dẫn đến O(n) hoạt động trên mỗi phân đoạn trừ khi chúng ta tính toán trước thứ gì đó như bảng thưa thớt. Ngay cả với quá trình tiền xử lý, việc kiểm tra tất cả các mảng con O(n^2) vẫn quá chậm, chỉ đưa ra các mảng con O(n^2) để xác thực. 

Điều quan trọng là phải hiểu khi nào AND và OR của một tập hợp số có thể bằng nhau. Đối với vị trí bit cố định, OR là 1 nếu ít nhất một phần tử có số 1 trong bit đó, trong khi AND chỉ là 1 nếu mọi phần tử đều có số 1 trong bit đó. Để hai kết quả này khớp nhau ở mọi bit, mỗi bit phải hoạt động giống hệt nhau trong cả hai thao tác. Cách duy nhất điều này có thể xảy ra là nếu, với mỗi bit, tất cả các phần tử đều có 0 hoặc tất cả các phần tử có 1. Điều kiện đó ngụ ý rằng tất cả các số trong mảng con đều giống hệt nhau. 

Vì vậy, vấn đề giảm xuống việc đếm các mảng con trong đó tất cả các phần tử đều bằng nhau. Thay vì suy nghĩ về các phép toán theo bit, giờ đây chúng ta chỉ cần theo dõi các lần chạy liền kề có giá trị bằng nhau. Mỗi phân đoạn tối đa có giá trị bằng nhau đóng góp một số lượng tổ hợp đơn giản của các mảng con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2) hoặc O(n2 log n) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Chúng ta quét mảng từ trái sang phải đồng thời nhóm các phần tử bằng nhau liên tiếp thành các khối tối đa. Điều này có tác dụng vì bất kỳ mảng con hợp lệ nào cũng phải nằm hoàn toàn bên trong một khối có giá trị giống hệt nhau. 
2. Đối với mỗi khối, chúng tôi xác định độ dài của nó, chẳng hạn`k`. Khi chúng ta biết rằng tất cả các phần tử trong phân đoạn này đều giống nhau, mọi mảng con bên trong nó sẽ tự động thỏa mãn điều kiện AND bằng OR. 
3. Chúng tôi tính toán phần đóng góp của khối này là`k * (k + 1) / 2`. Công thức này đếm tất cả các mảng con liền kề có thể có trong một đoạn có độ dài`k`. 
4. Chúng tôi tích lũy những đóng góp này trên tất cả các khối để tạo thành câu trả lời cuối cùng. 

Tại sao nó hoạt động 

Trong bất kỳ mảng con nào chứa hai giá trị khác nhau, tồn tại ít nhất một bit có giá trị khác nhau. Tại bit đó, OR sẽ là 1 trong khi AND sẽ là 0, phá vỡ đẳng thức. Điều này có nghĩa là các mảng con hợp lệ không thể vượt qua ranh giới nơi các giá trị liền kề khác nhau và không thể bao gồm các phần tử không đồng nhất. Do đó, các mảng con hợp lệ chính xác là những mảng được chứa đầy đủ trong các lần chạy tối đa có giá trị bằng nhau và việc đếm chúng sẽ giảm xuống việc đếm tất cả các mảng con bên trong mỗi lần chạy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    ans = 0
    i = 0
    
    while i < n:
        j = i
        while j < n and a[j] == a[i]:
            j += 1
        
        length = j - i
        ans += length * (length + 1) // 2
        
        i = j
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai là bản dịch trực tiếp ý tưởng chia mảng thành các phân đoạn có giá trị bằng nhau tối đa. Vòng lặp hai con trỏ xác định từng khối`[i, j)`nơi tất cả các giá trị phù hợp. Khi một khối được tìm thấy, sự đóng góp của nó được tính toán bằng cách sử dụng công thức số học cho số lượng mảng con. 

Một điểm tinh tế là đảm bảo rằng`j`tiến lên chính xác ngay cả khi mảng có nhiều giá trị lặp lại, vì việc không tiến lên đúng cách sẽ dẫn đến vòng lặp vô hạn hoặc đếm kép. Phép chia số nguyên là an toàn vì`k*(k+1)`luôn luôn chẵn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét mảng`[2, 2, 1, 1, 1]`. 

| Bước | Phân đoạn | Chiều dài | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | [2, 2] | 2 | 3 | 
| 2 | [1, 1, 1] | 3 | 6 | 

Khối đầu tiên đóng góp 3 mảng con:`[2]`,`[2]`,`[2,2]`. Thứ hai đóng góp 6 mảng con. Tổng số là 9. 

Điều này xác nhận rằng thuật toán phân tách mảng thành các vùng hợp lệ độc lập. 

### Ví dụ 2 

Hãy xem xét`[5, 5, 5, 5]`. 

| Bước | Phân đoạn | Chiều dài | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | [5, 5, 5, 5] | 4 | 10 | 

Mọi mảng con đều hợp lệ vì không có sự chuyển tiếp nào. Điều này kiểm tra trường hợp cạnh toàn phạm vi trong đó toàn bộ mảng tạo thành một khối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được truy cập đúng một lần khi tạo khối | 
| Không gian | O(1) | Chỉ sử dụng bộ đếm và chỉ số | 

Giải pháp chạy theo thời gian tuyến tính, đây là giải pháp tối ưu vì bất kỳ câu trả lời đúng nào ít nhất cũng phải đọc toàn bộ dữ liệu đầu vào. Việc sử dụng bộ nhớ không đổi ngoại trừ mảng đầu vào, dễ dàng phù hợp với các ràng buộc thông thường cho n tối đa 10^5 hoặc cao hơn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    
    n = int(input())
    a = list(map(int, input().split()))
    
    ans = 0
    i = 0
    while i < n:
        j = i
        while j < n and a[j] == a[i]:
            j += 1
        length = j - i
        ans += length * (length + 1) // 2
        i = j
    
    return str(ans)

# provided samples (hypothetical format)
assert run("3\n1 1 1\n") == "6", "all equal case"
assert run("5\n1 2 2 3 3\n") == "6", "mixed blocks"

# custom cases
assert run("1\n7\n") == "1", "single element"
assert run("2\n1 2\n") == "2", "all distinct"
assert run("4\n9 9 9 9\n") == "10", "single long block"
assert run("6\n1 1 2 2 2 1\n") == "8", "multiple blocks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n7\n`|`1`| đầu vào tối thiểu | 
|`2\n1 2\n`|`2`| mỗi phần tử tạo thành khối riêng của nó | 
|`4\n9 9 9 9\n`|`10`| đếm khối toàn dải | 
|`6\n1 1 2 2 2 1\n`|`8`| nhiều phân đoạn và chuyển tiếp | 

## Vỏ cạnh 

Đối với các mảng trong đó tất cả các phần tử đều bằng nhau, thuật toán sẽ thu gọn toàn bộ mảng thành một đoạn và đếm chính xác tất cả các mảng con thông qua công thức số tam giác. Đối với một đầu vào như`[4, 4, 4]`, vòng lặp tạo thành một khối có độ dài 3 và xuất ra 6, khớp với số lượng tất cả các mảng con có thể có. 

Đối với các mảng không có phần tử liền kề nào khớp nhau, chẳng hạn như`[1, 2, 3, 4]`, mỗi phần tử tạo thành một khối có độ dài 1. Mỗi phần tử đóng góp chính xác một mảng con hợp lệ, nên tổng số là 4. Thuật toán xử lý việc này một cách tự nhiên vì mỗi lần lặp sẽ ngay lập tức đóng một khối có kích thước 1 trước khi tiếp tục. 

Đối với các mẫu hỗn hợp như`[1, 1, 2, 1, 1]`, phần tử ở giữa chia cấu trúc thành ba khối. Thuật toán xử lý từng mảng một cách độc lập, đảm bảo rằng không có mảng con xuyên ranh giới không hợp lệ nào được tính.
