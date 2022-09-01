1. What database changes would be required to the starter code to allow for different roles for authors of a blog post? Imagine that we’d want to also be able to add custom roles and change the permission sets for certain roles on the fly without any code changes.
    - First, we should create an additional table, `post_role`, that will contain the set of possible roles as well as the permissions that each role entails.
    Each permission is set as a boolean flag. This allows for future roles, permissions, and for the permissions of a role to be changed on the fly through editing this table.

    - For example,
    ```
    const PostRole = db.define(
        'post_role',
        {
            roleId: {
                type: Sequelize.INTEGER,
                allowNull: false,
                autoIncrement: true,
                primaryKey: true
            },
            name: {
                type: Sequelize.STRING,
                allowNull: false
            },
            description: {
                type: Sequelize.STRING,
                allowNull: false
            },
            canModifyAuthors: {
                type: Sequelize.BOOLEAN,
                allowNull: false,
                defaultValue: false
            },
            canEditPost: {
                type: Sequelize.BOOLEAN,
                allowNull: false,
                defaultValue: false
            }
        },
        {
            timestamps: false,
            underscored: true,
            freezeTableName: true,
        }
    );
    ```
    - We would then add an additional column, `user_role`, to the `user_post` table that would store the roleId for the role that the user has on that post.
    All current `user_post` entries can be converted to owners.


2. How would you have to change the PATCH route given your answer above to handle roles?
    - It depends on if we're allowed to make breaking changes to the API.
        If we are, then I would prefer to delete the `authorIds` property from the request and create an `authors` property,
        an array of Author objects, where each object is of the form
        ```
        {
            id: 1, # number,
            role: "owner", # string
        }
        ```
        If we are not allowed to delete authorIds, then we can keep accepting authorIds with a sensible default role such as viewer.
        If the author already exists, they should keep their current role. Either way, I would like to create the authors property to allow for updating someone's role.

    - In code, to validate that the user has permission to modify the list of authors (and their roles), we would check that the user's `PostRole` has the permission flag `canModifyAuthors` set to true.
    - We would also need to validate that the new set of `UserPost`s has at least one with the `canModifyAuthors` flag set to true.