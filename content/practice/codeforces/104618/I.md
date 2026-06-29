---
title: "CF 104618I - Rắc ma thuật"
description: "Chúng ta có một tập hợp các điểm trong mặt phẳng, mỗi điểm biểu thị một điểm đặt ở đâu đó phía trên trục x, vì tất cả các tọa độ y đều dương. Mỗi lần rắc có một màu, đỏ hoặc xanh."
date: "2026-06-29T17:31:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104618
codeforces_index: "I"
codeforces_contest_name: "UTPC Contest 09-22-23 Div. 1"
rating: 0
weight: 104618
solve_time_s: 74
verified: false
draft: false
---

[CF 104618I - Rắc ma thuật](https://codeforces.com/problemset/problem/104618/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm trong mặt phẳng, mỗi điểm biểu thị một điểm đặt ở đâu đó phía trên trục x, vì tất cả các tọa độ y đều dương. Mỗi lần rắc có một màu, đỏ hoặc xanh. Bob đứng cố định tại điểm gốc và chọn một vùng hình học có dạng hình tròn với đỉnh ở gốc tọa độ. Vùng này không bị giới hạn về bán kính nhưng bị giới hạn trong một khoảng góc, do đó về mặt hình học, nó tương ứng với việc chọn hai tia từ gốc tọa độ và lấy tất cả các điểm giữa chúng theo thứ tự ngược chiều kim đồng hồ. 

Rắc được thu thập nếu nó nằm bên trong hoặc trên ranh giới của khu vực này. Bob muốn chọn một khu vực không chứa điểm màu xanh nào cả, đồng thời tối đa hóa số điểm màu đỏ bên trong nó. 

Kích thước đầu vào có thể đạt tới 200.000 điểm, do đó, bất kỳ phương pháp nào kiểm tra tất cả các cặp điểm hoặc tất cả các khoảng góc một cách rõ ràng sẽ thất bại. Một giải pháp bậc hai sẽ yêu cầu cân nhắc khoảng 40 tỷ cặp trong trường hợp xấu nhất, vượt xa mức cho phép trong 2 giây. Điều này ngay lập tức gợi ý rằng bài toán phải giảm xuống cấu trúc một chiều, rất có thể liên quan đến việc sắp xếp góc xung quanh gốc tọa độ. 

Một vấn đề tế nhị đến từ hình học trên một vòng tròn. Mặc dù tất cả các điểm đều nằm ở nửa mặt phẳng trên nhưng các góc bao quanh trục x âm. Bất kỳ giải pháp nào cũng phải xử lý cẩn thận thứ tự vòng tròn của các điểm theo góc. 

Một trường hợp cạnh khác xuất hiện khi các điểm màu xanh nằm giữa các cụm màu đỏ theo thứ tự góc. Một cách tiếp cận ngây thơ chỉ đơn giản là lấy khối điểm đỏ liên tiếp tối đa trong sắp xếp theo góc là sai vì nó có thể bao gồm các điểm màu xanh bên trong khoảng hoặc bỏ sót rằng khu vực tối ưu có thể bắt đầu và kết thúc tại các điểm tùy ý. 

Cạm bẫy thứ ba là coi khu vực này như một khoảng tuyến tính mà không xử lý trường hợp bao quanh. Khu vực tối ưu có thể cắt theo hướng dương của trục x, nghĩa là các khoảng góc có thể cần được nhân đôi để mô phỏng tính liên tục của vòng tròn. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi mọi cặp điểm có thể có là ranh giới của khu vực. Đối với mỗi cặp điểm có thứ tự, chúng ta tưởng tượng một cung bắt đầu ở góc thứ nhất và kết thúc ở góc thứ hai theo thứ tự ngược chiều kim đồng hồ, sau đó đếm xem có bao nhiêu điểm màu đỏ nằm bên trong trong khi đảm bảo không bao gồm các điểm màu xanh lam. Điều này yêu cầu kiểm tra tất cả các điểm cho từng cặp, dẫn đến độ phức tạp về thời gian O(N^3) hoặc O(N^2) nếu việc đếm được tối ưu hóa bằng quá trình tiền xử lý nhưng vẫn quá lớn đối với 200.000 điểm. 

Quan sát quan trọng là ranh giới khu vực có ý nghĩa duy nhất xảy ra ở các góc được xác định bởi các điểm đã cho. Nếu chúng ta cố định một ranh giới và xoay ranh giới kia, điều kiện “không có điểm màu xanh bên trong” sẽ trở thành ràng buộc cửa sổ trượt theo thứ tự góc được sắp xếp. Điều này biến bài toán thành tìm khoảng góc hợp lệ dài nhất không chứa điểm xanh và cực đại hóa các điểm đỏ. 

Sau khi tất cả các điểm được chuyển đổi thành các góc cực, chúng ta có thể sắp xếp chúng và nhân đôi mảng để xử lý việc bao quanh. Sau đó, bài toán trở thành việc chọn một đoạn liền kề trong mảng hình tròn nhân đôi này sao cho nó không chứa điểm màu xanh và số điểm màu đỏ là lớn nhất. Quét hai con trỏ duy trì một cửa sổ không có điểm màu xanh lam; bất cứ khi nào một điểm màu xanh đi vào cửa sổ, chúng ta di chuyển con trỏ bên trái về phía trước cho đến khi ràng buộc được khôi phục. 

Trong mỗi cửa sổ hợp lệ, chúng tôi theo dõi số lượng điểm đỏ được bao gồm và lấy mức tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các cặp ngành | O(N^3) | O(N) | Quá chậm | 
| Sắp xếp + hai con trỏ trên các góc | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mỗi điểm (x, y) thành một góc cực bằng cách sử dụng atan2(y, x).

Điều này đảm bảo thứ tự góc chính xác xung quanh điểm gốc, tôn trọng cấu trúc góc phần tư. 
2. Ghép từng góc với màu của nó và sắp xếp danh sách theo góc. 

Sắp xếp tuyến tính hóa hình tròn thành một chuỗi. 
3. Nhân đôi mảng đã sắp xếp bằng cách thêm (góc + 2π, màu) cho mỗi phần tử. 

Điều này cho phép các khoảng bao quanh được biểu diễn dưới dạng các mảng con thông thường. 
4. Khởi tạo hai con trỏ l và r ở đầu mảng nhân đôi và duy trì một cửa sổ. 

Cửa sổ đại diện cho khu vực góc ứng cử viên hiện tại. 
5. Mở rộng r từng bước, thêm các điểm vào cửa sổ. 

Mỗi lần chúng tôi thêm một điểm, chúng tôi sẽ cập nhật số điểm màu đỏ và xanh lam. 
6. Nếu một điểm màu xanh lam xuất hiện trong cửa sổ, hãy di chuyển l về phía trước cho đến khi không còn màu xanh lam. 

Điều này khôi phục điều kiện hợp lệ rằng khu vực đó phải chứa 0 điểm màu xanh lam. 
7. Bất cứ khi nào cửa sổ hợp lệ, hãy tính số điểm màu đỏ bên trong và cập nhật câu trả lời. 

Khu vực tốt nhất phải tương ứng với một số cặp ranh giới cửa sổ hợp lệ tối đa. 
8. Đảm bảo rằng độ dài cửa sổ không bao giờ vượt quá N, vì một khu vực hợp lệ không thể bao gồm nhiều hơn một vòng quay đầy đủ các điểm duy nhất. 

### Tại sao nó hoạt động 

Sau khi sắp xếp theo góc, mọi khu vực hợp lệ sẽ tương ứng với một khoảng liền kề nào đó trên vòng tròn. Việc sao chép mảng sẽ chuyển đổi các khoảng tròn thành các khoảng tuyến tính mà không làm mất bất kỳ khu vực ứng cử viên nào. Quá trình hai con trỏ duy trì tính bất biến là cửa sổ hiện tại không chứa điểm màu xanh lam và bất cứ khi nào một điểm màu xanh đi vào, ranh giới bên trái sẽ được nâng lên vừa đủ để loại bỏ nó. Điều này đảm bảo rằng mọi khoảng góc hợp lệ tối đa được coi là chính xác một lần khi con trỏ bên phải mở rộng, do đó số điểm màu đỏ tối đa trong số tất cả các khu vực không có màu xanh lam hợp lệ sẽ được ghi lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math

def main():
    n = int(input())
    colors = input().strip()
    
    pts = []
    for i in range(n):
        x, y = map(int, input().split())
        ang = math.atan2(y, x)
        pts.append((ang, colors[i]))
    
    pts.sort()
    
    arr = pts + [(ang + 2 * math.pi, col) for ang, col in pts]
    
    l = 0
    red = 0
    blue = 0
    ans = 0
    
    for r in range(len(arr)):
        if arr[r][1] == 'r':
            red += 1
        else:
            blue += 1
        
        while blue > 0:
            if arr[l][1] == 'r':
                red -= 1
            else:
                blue -= 1
            l += 1
        
        ans = max(ans, red)
        
        if r - l + 1 > n:
            if arr[l][1] == 'r':
                red -= 1
            else:
                blue -= 1
            l += 1
    
    print(ans)

if __name__ == "__main__":
    main()
```Việc thực hiện trực tiếp theo sau việc xây dựng quét góc. Việc chuyển đổi atan2 rất quan trọng vì nó duy trì thứ tự chính xác trên tất cả các góc phần tư. Việc sắp xếp đảm bảo rằng bất kỳ khu vực hợp lệ nào cũng sẽ trở thành một khoảng liền kề trong không gian góc. 

Bước sao chép là bước cho phép xử lý toàn diện. Không có nó, một se
