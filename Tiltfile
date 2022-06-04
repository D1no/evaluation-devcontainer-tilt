
# version_settings() enforces a minimum Tilt version
# https://docs.tilt.dev/api.html#api.version_settings
version_settings(constraint='>=0.22.2')

# =========

# Environment: Check if we are in github code spaces, to enable extra endpoint links.
if "CODESPACE_NAME" in os.environ:
    CODESPACE = {
        "domain": "githubpreview.dev",
        "name": os.environ["CODESPACE_NAME"]
    }
else:
    CODESPACE = False

def k8s_resource_add_codespace_link(resource_name, port, label = "Container Endpoint"):
    """Adds a Codespace endpoint link to an k8s resource, since remote containers are not running on localhost."""
    if resource_name != None and port != None and CODESPACE != False:

        # Constructed URL. Ports are part of the sub-domain on github codespaces.
        url = "https://" + CODESPACE["name"] + "-" + str(port) + "." + CODESPACE["domain"]

        print("📝 Codespace Info: UI link for k8s resource >" + resource_name + "< on Port >" + str(port) + "< called: " + label)
        print(" ↳ " + url)
        k8s_resource(
            resource_name,
            links=[
                link(url, label)
            ]
        )

# =========

# tilt-avatar-api is the backend (Python/Flask app)
# live_update syncs changed source code files to the correct place for the Flask dev server
# and runs pip (python package manager) to update dependencies when changed
# https://docs.tilt.dev/api.html#api.docker_build
# https://docs.tilt.dev/live_update_reference.html
docker_build(
    'tilt-avatar-api',
    context='.',
    dockerfile='./deploy/api.dockerfile',
    only=['./api/'],
    live_update=[
        sync('./api/', '/app/api/'),
        run(
            'pip install -r /app/requirements.txt',
            trigger=['./api/requirements.txt']
        )
    ]
)

# k8s_yaml automatically creates resources in Tilt for the entities
# and will inject any images referenced in the Tiltfile when deploying
# https://docs.tilt.dev/api.html#api.k8s_yaml
k8s_yaml('deploy/api.yaml')

# k8s_resource allows customization where necessary such as adding port forwards and labels
# https://docs.tilt.dev/api.html#api.k8s_resource
k8s_resource(
    'api',
    port_forwards='5734:5000',
    labels=['backend']
)

k8s_resource_add_codespace_link("api", 5734, "API URL (Codespaces)")


# tilt-avatar-web is the frontend (ReactJS/vite app)
# live_update syncs changed source files to the correct place for vite to pick up
# and runs yarn (JS dependency manager) to update dependencies when changed
# if vite.config.js changes, a full rebuild is performed because it cannot be
# changed dynamically at runtime
# https://docs.tilt.dev/api.html#api.docker_build
# https://docs.tilt.dev/live_update_reference.html
docker_build(
    'tilt-avatar-web',
    context='.',
    dockerfile='./deploy/web.dockerfile',
    only=['./web/'],
    ignore=['./web/dist/'],
    live_update=[
        fall_back_on('./web/vite.config.js'),
        sync('./web/', '/app/'),
        run(
            'yarn install',
            trigger=['./web/package.json', './web/yarn.lock']
        )
    ]
)

# k8s_yaml automatically creates resources in Tilt for the entities
# and will inject any images referenced in the Tiltfile when deploying
# https://docs.tilt.dev/api.html#api.k8s_yaml
k8s_yaml('deploy/web.yaml')

# k8s_resource allows customization where necessary such as adding port forwards and labels
# https://docs.tilt.dev/api.html#api.k8s_resource
k8s_resource(
    'web',
    port_forwards='5735:3000',
    labels=['frontend'],
)

k8s_resource_add_codespace_link("web", 5735, "Open App (Codespaces)")



# config.main_path is the absolute path to the Tiltfile being run
# there are many Tilt-specific built-ins for manipulating paths, environment variables, parsing JSON/YAML, and more!
# https://docs.tilt.dev/api.html#api.config.main_path
tiltfile_path = config.main_path

# print writes messages to the (Tiltfile) log in the Tilt UI
# the Tiltfile language is Starlark, a simplified Python dialect, which includes many useful built-ins
# config.tilt_subcommand makes it possible to only run logic during `tilt up` or `tilt down`
# https://github.com/bazelbuild/starlark/blob/master/spec.md#print
# https://docs.tilt.dev/api.html#api.config.tilt_subcommand
if config.tilt_subcommand == 'up':
    print("""
    \033[32m\033[32mHello World from tilt-avatars!\033[0m

    If this is your first time using Tilt and you'd like some guidance, we've got a tutorial to accompany this project:
    https://docs.tilt.dev/tutorial

    If you're feeling particularly adventurous, try opening `{tiltfile}` in an editor and making some changes while Tilt is running.
    What happens if you intentionally introduce a syntax error? Can you fix it?
    """.format(tiltfile=tiltfile_path))
